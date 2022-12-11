A simple anomaly detection algorithm based on dense optical flow.

1. Get current flow, an array of depth 2 (angle, magnitude).
2. Transform into an array of depth d, where the depth signifies a range of angles and the value signifies magnitude. 
	1. e.g. a angle of 160 degrees with d=4 would give a depthwise slice of (0, mag, 0, 0)
3. Update a baseline array (same shape as in 2) which captures the moving average of this array.
4. Detect anomalous inputs arrays by comparing to this baseline

#computervision 