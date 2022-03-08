Named pipes / FIFOs provide a queue mechanism that is more performant than the Python implementation of queue or pipe. The native method uses python's "pickle" serialisation to pass objects between processes, which conveniently allows any Python object to be passed. However, the process of pickling the object is computationally intensive and renders queues useless for large objects such as image arrays.

Using os level named pipes or FIFOS is much more performant, but requires the developer to convert their message to bytes and restricts max message length to the system page size.

#programming-paradigm 

This can be useful when the developer needs low level control over the multiprocessing implementation, unlike in [[ReactiveX - asynchronous programming API]] where these details are abstracted away.

### Sample code
```
import os
import sys
import traceback
import time
import select
import json

from multiprocessing import Process
import logging

#UTILS 
def encode_msg_size(size: int) -> bytes:
    return struct.pack("<I", size)

def decode_msg_size(size_bytes: bytes) -> int:
    return struct.unpack("<I", size_bytes)[0]

def create_msg(content: bytes) -> bytes:
    size = len(content)
    return encode_msg_size(size) + content

def get_message(fifo: int) -> str:
    """Get a message from the named pipe."""
    msg_size_bytes = os.read(fifo, 4)
    msg_size = decode_msg_size(msg_size_bytes)
    msg_content = os.read(fifo, msg_size).decode("utf8")
    return msg_content

def get_logger():
    logger = multiprocessing.get_logger()
    logger.setLevel(logging.INFO)
    formatter = logging.Formatter(\
                                  '[%(asctime)s| %(levelname)s| %(processName)s]| %(filename)s:%(lineno)s| %(message)s')
    handler = logging.FileHandler('opt/aws/panorama/logs/my_app.log')
    handler.setFormatter(formatter)

    # this bit will make sure you won't have
    # duplicated messages in the output
    if not len(logger.handlers):
        logger.addHandler(handler)
    return logger


log = get_logger()

class PostProcessProcess(Process):
    def __init__(self, fifoName, destination):
        super(PostProcessProcess, self).__init__()
        #Setup
        self.fifoName = fifoName
        os.mkfifo(self.fifoName)

    def run(self):
        #Start FIFO
        self.fifo = os.open(self.fifoName, os.O_RDONLY | os.O_NONBLOCK)
        self.poll = select.poll()
        self.poll.register(self.fifo, select.POLLIN)

        while True:
            #Main loop
            self.read_fifo_and_process() #Read data from fifo, process data 

    def read_fifo_and_process(self):
        """Read data from fifo, process data inc. tracking, append message buffer"""
        try:
            if(self.fifo, select.POLLIN) in self.poll.poll(1000):
                msg = get_message(self.fifo)
                item = json.loads(msg)
                ### DO OTHER ACTIONS HERE ###
        except Exception as err:
            log.exception("Exception reading from fifo", err)

            
class PostProcessor():
    def __init__(self):
        ipc_fifo_name = POSTPROC_IPC_FIFO_NAME + str(name_clean)
        #Setup FIFO pipe
        if os.path.exists(ipc_fifo_name):
            log.info('IPC file exists, deleting')
            os.remove(ipc_fifo_name)
        try:
            #Start process
           self.post_process = PostProcessProcess(ipc_fifo_name, destination)
           self.post_process.start()
        except Exception as err:
            log.exception(err)
        time.sleep(2) #wait for FIFO to be created by the process
        retries = 1
        while(not os.path.exists(ipc_fifo_name)):
            time.sleep(1) #wait for FIFO to be created by the process
            retries += 1
            if retries % 60 == 1:
                log.exception("POSTPROC_IPC_FIFO_NAME does not exist")
        #Open FIFO pipe
        self.fifo = os.open(ipc_fifo_name, os.O_WRONLY)
        log.info("Post Processor initialised,"+str(destination)) #Log success
    
    def queue_inference_json_list(self, items):
        for item in items:
            self._queue_inference_string(item)
    
    def _queue_inference_string(self, message):
        # return None
        msg = create_msg(json.dumps(message, default=str).encode('utf-8'))
        try:
            os.write(self.fifo, msg)
        except Exception as err:
            log.exception('Unable to write to fifo. Message length:' + str(len(msg)), err,)
            time.sleep(5)
    
    def terminate(self):
        self.post_process.join()
```