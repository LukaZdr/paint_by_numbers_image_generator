# Paint by Number Image Generator (WIP)
This Jupyter Notebook generates paint by number images and color palettes for a given input image.
This Notebook is a little recap on some methods for image processing that I have learned.

https://colab.research.google.com/drive/1Qt_ZULUgOXAgHaQq0x-lRRjNuc6yrse7?usp=sharing.

In the process of generating a paint by number image, the image must first be segmented into superpixels. For this, the SLIC-Superpixel algorithm was chosen. This example shows an image that has been split into n=3000 superpixels. These superpixels are then all colored with their respective mean RGB value.

![image](https://user-images.githubusercontent.com/24440000/148657964-cbff8eae-37db-417d-9279-0c90fcc7d451.png) --> ![image](https://user-images.githubusercontent.com/24440000/148659289-b42e4348-2125-43b9-aae2-2a3ce7226b2c.png)

One immediate problem is that there are a lot of small superpixels that are close to each other and have a similar color. These can be merged by constructing a Region Adjacency Graph (RAG). The rag can help visualize and merge similar superpixels that share a boarder. All region that are under a specific threshold of difference get merged to reduce complexity and the superpixelcount.

![image](https://user-images.githubusercontent.com/24440000/148659405-f4ad4331-ba39-48ef-9959-5b302ea4c616.png) --> ![image](https://user-images.githubusercontent.com/24440000/148659413-bff9e731-e88c-4b9c-8b13-0432b3331b75.png)

Since this can still result in potentially n different colors, there is a need to also simplify the color space. For this, all present colors of the superpixels are treated with the K-means Clustering Algorithm. The desired number of colors used to paint the resulting picture can be chosen here. For each resulting color, a cluster center needs to be created.
After training the algorithm on the present RGB colors, the resulting cluster centers can be used to repaint all RGB pixel values in their cluster with the value of that center.

![image](https://user-images.githubusercontent.com/24440000/148658177-a84b4bca-8619-44d2-b563-7522c309fde5.png) --> ![image](https://user-images.githubusercontent.com/24440000/148658564-fe5ec32b-e5e1-466b-93fc-011714a52ed0.png)

This further simplifies the image without losing important features of the image. Now that the image is much simpler, a color palette can be visualized. These colors are the Cluster centers from before.

![image](https://user-images.githubusercontent.com/24440000/148658651-e879d27c-e406-4372-b35b-80b028274660.png)

At last (for now) the boarders off the simplified image can be visualized so that the image can be painted.

![image](https://user-images.githubusercontent.com/24440000/148659600-e3dde54f-375e-45db-bd78-067b2ec611c1.png)

Next steps:
- order the colors in the palette and give them numbers
- write these numbers into each superpixel
- generate a printable PDF with the palette and the to be painted image
