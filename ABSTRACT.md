Authors present a procedure to automatically generate an high-quality training dataset of cablelike objects for semantic segmentation. The proposed method is explained in detail using the recognition of electric wires as a use case. These particular objects are commonly used in an extremely wide set of industrial applications, since they are of information and communication infrastructures, they are used in construction, industrial manufacturing and power distribution. The proposed approach uses an image of the target object placed in front of a monochromatic background. By employing the chroma-key technique, we can easily obtain the training masks of the target object and replace the background to produce a domain-independent dataset. How to reduce the reality gap is also investigated in this work by correctly choosing the backgrounds, augmenting the foreground images exploiting masks. The produced dataset is experimentally validated by training two algorithms and testing them on a real image set. Moreover, they are compared to a baseline algorithm specifically designed to recognise deformable linear objects.

## Auto-labeling with Chroma Key

The Chroma Key (CK) is a technique widely used in film and motion picture industries to combine two images together (usually foreground and background). It requires a foreground image containing a target object that we want to overlap to a background image.  The target must be placed in front of a monochromatic panel, called screen (usually green or blue). The technique consists of a chroma-separation phase, where authors isolate the target object (foreground) from the monochromatic panel (original background) and then an image-overlay phase, where they compose the foreground and a
new background. In the chroma-separation phase, authors choose
a specific hue range which contains solely the color of the screen (e.g. green) and exclude any other color belonging to the foreground. Then, by finding the pixels within that range, authors obtain a mask for the target Imt and a complementary mask for the monochromatic background Ims. Thus, creating a dataset with this technique is really straightforward and it can be done in 2 steps:
1. Record an high quality video of the target object on a
green screen, from which we gather the input images;
2. Find the chroma range of the pixels belonging to the
monochromatic background and create the corespondent
mask with chroma separation.

In our dataset, while gathering the images, we hold the electric wire by its extremities and we move it within the frame composing different shapes. To generalize more we also change the light setups, the wire color and the number of wire in the scene. From a random video frame we easily find the hue levels for the specific screen color we are using (green or blue). These levels, once found for one image, remain valid for any other image taken with the same light temperature setting and white balance. Hence, known the chroma range of the screen we immediately obtain the mask for the wire from each frame in the video.

## Domain Randomization

The labeling procedure with chroma separation automatically generates labeled data ready to be used for training, but with a low variability. In fact, in the gathering phase we need to randomize the scene featuring target object in the following aspects: number of instances, color, size, position and shape. Nevertheless, the background is always uniform and monochromatic. The performance of a segmentation algorithm trained with images in homogeneous backgrounds would be significantly degraded when working in a complex and chaotic environment. Clutter background in fact easily confuses the algorithm, due to possible similarities between the target and the background, especially if it has never seen them in training. This weakness can be readily overcome by replacing the background in the input images (image-overlay phase). In fact, by using the masks, we can combine the foreground with a random background that replaces the green screen. This process, known as domain randomization, aims to provide enough synthetic variability in training data such that at test time the model is able to generalize to real-world data. Hence, the choice of the background images is a key point for generalizing well to multiple real-world target domains without the need of accessing any target scenario data in training.

The backgrounds that we propose for a domain-independent dataset can be divided in 3 categories: lowly textured images with shadows and lights; highly textured images
with color gradients and regular or geometric shapes; highly textured images with chaotic and irregular shapes. These backgrounds introduce high variance in the environment properties that should be ignored in the learning task. For instance, in our task the segmentation algorithm will ignore shadows and cubic or spherical objects, while it should focus more in cylindrical shapes, hence we chose the set of backgrounds in Figure 2 according to these considerations.

This strategy has been employed to generate a dataset of 28584 RGB images 720 × 1280 for semantic segmentation of electric wires. The raw dataset has 3176 images and it includes blue, red, yellow, white and black wires, with different light setups and shapes. To improve the screen and wire separation, besides the hue, we also use the saturation and value channels. For each raw image, a background image (4000 × 2248) is randomly picked among the 15 shown in Figure 2 and 8 new synthetic images are created, as visible in Figure 1. In each new image, foreground and background are separately augmented (by using the mask) before the merging. In particular, the background is randomly flipped, shifted, scaled and rotated (all with probability <i>p = 0.5</i>). Then, it is processed with motion blur and elastic transformation (<i>p = 0.2</i>), and in the end it is randomly cropped at 1280×720 (<i>p = 1</i>). The foreground, instead, is transformed only by shuffling the channels (<i>p = 0.5</i>), converting to grey (<i>p = 0.1</i>) and randomizing the hue in the range of [−100, 100] (<i>p = 0.5</i>).

![Fig1](https://i.ibb.co/VLz3JHs/Screenshot-2023-09-25-192921.png)

<i>Fig. 1: Schematic process to generate the 8 synthetic images by background-foreground separated augmentation and imageoverlay</i>

![Fig2](https://i.ibb.co/gPV8M22/Screenshot-2023-09-25-192952.png)

<i>Fig. 2: Images used to replace the background in the output dataset.</i>



