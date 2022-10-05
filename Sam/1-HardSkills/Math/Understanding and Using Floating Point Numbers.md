# Understanding and Using Floating Point Numbers	
> One of the wonderful things about programming is that nearly everywhere you look, you find more than meets the eye.

## I. Accuracy vs. Precision
![](large.jpg)
"Accuracy" refers to how close a measurement is to the true value.
	 "Precision" has to do qwith how much information you have about a quantity, how uniquely you have it pinned down.
 
Integer arithmetic enjoys the property of complete accuracy: if I have the integer "2", it is exactly 2, on the dot, and nobody can dispute that.

However, integers lack precision.  Dividing both 5 and 4 by 2, for instance, will both yield 2. Integers are unable to keep track of the fractional part, so the information that I had a slightly bigger number than 4 (namely, 5) is lost in the process. Integers are too "chunky" to represent these finer gradations;