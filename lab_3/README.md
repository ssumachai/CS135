# CS135 Winter 2022 - Lab 3

Lab 3 was a Collaboration between Sumachai Suksanguan (ssuks001) and Diego Vega (dvega)

## Parts and Explanation

All Assignments were completed and tested within the online environment

### Part A

Part A was done by simply commenting out the line of code responsible for resetting the cubes current
position back to it's original.  The cubes' position is constantly being updated throughout the duration
of the squeeze movement, so once squeeze ends, it's final position should simply just be left where it is.

### Part B

Part B was done after realizing that in the function `onSelectStart(ev)` there exists a conditional statement
that checks to see if the target we "hit" is one of our target boxes.  Since we are creating a box in
the space where one shouldn't exist, all we needed to do was add an `else` statement to that if-statement:

```html
else{
	let TargetRay = new Ray(targetRayPose.transform.matrix);
	addBox(targetRay.origin[0], targetRay.origin[1], targetRay.origin[2], 255, 255, 255);
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
