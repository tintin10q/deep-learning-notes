# R-CNN 

The idea of an R-CNN is that you first train the [CNN](CNN) to detect certain things in an image like a cat or dog or bike. Than you split up an image into tiny squares and you run all those squares through the ccn. Then you know which parts of the images are actually a dog or a bike. 

## Fast R-CNN

You make a single cnn that can do everything in one go. 
You also do some search to decide which cells of the image to search first. 

There is also the faster R-CNN with better search algoritm. It has a region proposal function to propose good regions to look at.

## YOLO

You only look once. This is that you only run a cell through another cnn if it hasn't been classified yet. This way your not running over a part of the image again. 

