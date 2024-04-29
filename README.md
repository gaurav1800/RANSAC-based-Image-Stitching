# RANSAC-based-Image-Stitching
Discover Python's RANSAC-based Image Stitching project for seamless panoramas. Explore robust stitching algorithms for efficient image fusion. Dive into computer vision with practical Python implementation.

**Process of making the panorama with the use of an affine transformation:**
1. **Preprocessing** Load both images and convert to grayscale.

2. **Detect keypoints and extract descriptors** Compute image features in both images. The feature detector and descriptor you will be using is SIFT. Use the OpenCV library to compute SIFT features. The function for computing SIFT features is cv2.SIFT create().detectAndCompute(); see this tutorial for details about using SIFT in OpenCV. To visualize the detected SIFT keypoints and their respective orientations in the image, use the function cv2.drawKeypoints.

3. **Match features** Compute distances between every SIFT descriptor in one image and every descriptor in the other image. For fast computation of (squared) Euclidean distances, you can use either scipy.spatial.distance.cdist(X,Y,‘sqeuclidean’) or kornia.feature.match snn(tensor1, tensor2).

4. **Prune features** Select the closest matches based on the matrix of pairwise descriptor distances obtained above. You can select all pairs whose descriptor distances are below a specified threshold, or select the top few hundred descriptor pairs with the smallest pairwise distances. Display the locations of these inlier matches in both images by using plot inlier matches (provided in the starter code).

5. **Robust transformation estimation** Implement RANSAC to estimate an affine transformation mapping one image to the other. Use the minimum number of pairwise matches to estimate the affine transformation; state this minimum number as a comment in your code. Since you are using the minimum number of pairwise points, the transformation should be estimated using the inverse transformation rather than least-squares. Inliers are defined as the number of transformed points from image 1 that lie within a user-defined radius of ρ pixels of their pair in image 2. You will need to experiment with the matching threshold, ρ, and the required number of RANSAC iterations.

6. **Compute optimal transformation** Using all the inliers of the best transformation found using RANSAC (i.e., the one with the most inliers), compute the final transformation with least-squares.

7. **Create panorama** Using the final affine transformation recovered using RANSAC, generate the mosaic and display the colour mosaic result to the screen; your result should be similar to the result in Fig. 2. Warp one image onto the other using the estimated transformation. To do this, you can use either the functions skimage.transform.ProjectiveTransform and skimage.transform.warp together or use kornia.geometry.warp perspective. Create a new image large enough to hold the mosaic and composite the two images into it. You can create the mosaic by taking the pixel with the maximum value from
each image. This tends to produce less artifacts than taking the average of warped images. To create a colour mosaic, apply the same compositing step to each of the colour channels separately.


**Process of creating a panorama with a homography transformation:**
1. Using the RANSAC-based homography code above, generate the mosaic using the given image pair and **display the colour mosaic result to the screen**.
  
2. Run the code on an image triplet of your own choosing and **display the colour mosaic result to the screen**. Make sure the images you choose have significant overlap; otherwise, you will not be able to establish correspondences. Further, for a homography to be valid, the images can either be obtained from rotating in the same place OR from multiple vantage points if the scene is planar or approximately planar.


**NOTES:**
**When running the code**: Modify the paths when reading the images depending on the location of the image path.
**Images**: Unstitched images are located in a directory named "Unstitched Images", and the stitched images or the final outputs, are within the ipynb file which can be opened using either Google Colab or Jupyter notebook (I used the latter).

