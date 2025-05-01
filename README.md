# Dilepton ML Code

The code attached in DileptonRingDetectionMLCode.ipynb is used to train a Machine Learning (ML) model which will distinguish between FG and BG radiation.



The FG radiation are the Cherenkov rings created by dilepton pairs which are simulated and extracted through a root file. To modify which root file is used, in the second box there are several lines of code which load the root file, as well as extract the x and y coordinates, as well as the GEANTID's of the rings.

## Image generation

The third and fourth boxes create the event images as scatter plots and save them as images to be loaded into the ML algorithm later. The fourth box creates dilepton rings as "masks" and are used as such in the algorithm. The masks are rings that are created without BG radiation.

## Image loading

The fifth and sixth boxes load the images and then zip them together so that the images of the generated events are paired properly with their corresponding rings.

## Training/Evaluation/Testing split

The seventh box splits the zipped images into training/eval/test batches, and the percentage can be changed by modifying the lines

train_size = int(0.2 * len(images))

val_size = int(0.1 * len(images))

## U-NET algorithm

The U-NET algorithm is defined by its capacity to strip images down by a factor of 2^4, and then reconstructing the image back up. For the ML algorithm to work properly, it must be ensured that the images are of such a size that they can be divided evenly by 2^4 at minimum.

For more information on the U-NET algorithm, see https://arxiv.org/pdf/1505.04597

## ML code

The rest of the code is used to run the machine learning algorithm.

Through trial and error, a loss function of tf.keras.losses.MAE was found to work the best at reconstructing rings.

## Plotting the ML output

The images are gotten through the line predictions = model.predict(test_images). This is outputted as an image, and thus to plot the output plt.imshow must be used. Predictions is outputted as a 3D array of images, and thus to plot a specific event it is necessary to plot predictions[0].