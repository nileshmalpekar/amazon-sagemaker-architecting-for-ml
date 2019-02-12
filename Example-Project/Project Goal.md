# Object Detection 
To detect presence of ship(s) using Airbus satellite image data
    - Image classification: Image contain
    - Image segmentation (stretch goal)

# Datasets

Airbus offers comprehensive maritime monitoring services by building a meaningful solution for wide coverage, fine details, intensive monitoring, premium reactivity and interpretation response. Airbus captures image data using satellite service which could be used for maritime monitoring.  

Currently, Airbus is producing more satellite data than it can process, and is interested in an automated solution.

3 Files are 
- train_v2.zip
    - N images with 768 x 768 pixel resolution
- test_v2.zip
    - N images with 768 x 768 pixel resolution
- train_ship_segmentations_v2.csv
    - This file contains the Run Length Encoded masks of ships in each image. If there are no ships, the EncodedPixel column is blank.
    - 192,556 images with
        - 125,161 images have no ships in it
        - 67,395 images have ship information along with bounding box information

# Modeling Strategy
- Data handling
    - The data exists on the Kaggle website. We are going to download the data locally and then upload it to an S3 bucket 'objectdetectchicagoteam2'

- train_ship_segmentations_v2.csv file contains Run Length Encoded masks. We are going to use presence of data to determine if an image contains ship or not
    + There could be multiple rows for a given image if it contains multiple ships. We'll ignore multiple rows for the same image for the 1st part

- Image classification problem
    We are going to use Sagemaker's image classification built-in algorithm to solve this problem.
    Considerign the time and resources available to us, we'll be using partial data from train_v2.zip file to train the model as two class classification problem, indicating SHIP or NO_SHIP. The output will also indicate the classification confidence.

# End Goal
Processing satellite images manually is time consuming and requires manual intervention. Airbus is certainly producing lot more satellite image data than it can handle. 

We would like to build a REST api endpoint such that satellite image can be sent to this REST endpoint and it can return the result as
    - whether the image contains ship or not
    - segmentation data indicating which pixels belong to the ship (stretch goal)
