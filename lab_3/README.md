# CS135 Winter 2022 - Lab 3

Lab 3 was a Collaboration between Diego Vega (dvega007) and Sumachai Suksanguan (ssuks001)

## Parts and Explanation

All Assignments were completed and tested within the online environment

### Part A

Part A was done by simply commenting out the line of code responsible for resetting the cubes current
position back to it's original.  The cubes' position is constantly being updated throughout the duration
of the squeeze movement, so once squeeze ends, it's final position should simply just be left where it is.

#### Original

```js
vec3.add(currently_grabbed_box.scale, currently_grabbed_box.scale, [0.25, 0.25, 0.25]);
currently_grabbed_box.position = currently_grabbed_box.originalPos;
```

#### Updated

```js
vec3.add(currently_grabbed_box.scale, currently_grabbed_box.scale, [0.25, 0.25, 0.25]);
//currently_grabbed_box.position = currently_grabbed_box.originalPos;
```

### Part B

Part B was done after realizing that in the function `onSelectStart(ev)` there exists a conditional statement
that checks to see if the target we "hit" is one of our target boxes.  Since we are creating a box in
the space where one shouldn't exist, all we needed to do was add an `else` statement to that if-statement:

```js
else{
	let TargetRay = new Ray(targetRayPose.transform.matrix);
	addBox(targetRay.origin[0], targetRay.origin[1], targetRay.origin[2], 255, 255, 255);
}
```

`TargetRay` takes the ray outputted by our controllers, then converts it into a matrix that can be used to determine the new location of the box by accessing it's member
variable `origin`, with [0, 1, 2] directly corresponding to it's x, y, and z coordinates.  The values {255, 255, 255} simply represent white in RGB.

### Part C

Part C took a little bit more time reading JS functions and libraries to make it as doable as possible.  It's important to note what classifies as an intersection.  The lab
document defines an intersection as any two cubes that are within 0.25 m of each other. To implement this, we decided to calculate the Euclidean distance between the two cubes
centers, and if they were less than 0.25 m, then the cubes are intersecting:

```js
if(vec3.dist(box1, box2) <= 0.25){
	//Do Intersection Stuff
};
```

Now, we had to iterate through the entire array of boxes, and compare every single element, with every single element. Doing so we simply used an embedded for-loop, but for
complexity sake, that can probably be optimized to be less expensive:

```js
for(let box1 = 0; box1 < boxes.length; box1++){
	for(let box2 = box1 + 1; box2 < boxes.length; box2++){
        	let box1l = vec3.fromValues(
                boxes[box1].position[0],
                boxes[box1].position[1],
                boxes[box1].position[2]
                );
                let box2l = vec3.fromValues(
                boxes[box2].position[0],
                boxes[box2].position[1],
                boxes[box2].position[2]
                );
                if(vec3.dist(box1l, box2l) <= 0.25){
                	let box1color = boxes[box1].renderPrimitive.uniforms;
                        let box2color = boxes[box2].renderPrimitive.uniforms;
                        box1color.baseColorFactor.value = [255, 255, 255, 1.0];
                        box2color.baseColorFactor.value = [255, 255, 255, 1.0];
                }
	}
}
```

The program will loop through all boxes, creating `vec3` objects out of box positions, then directly comparing them to another box to calculate Euclidean Distance.  If the
distance is less than 0.25 m, it will fulfill the intesection condition, which will then create a `boxNumbercolor` variable, which will change the boxes color to white.
