# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.


# code
`

`
Feel import cv2
import numpy as np
from matplotlib import pyplot as plt

# Load face image
face = cv2.imread(r"D:\DIP\workshop\My photo.jpg")

# Load sunglass image
glass = cv2.imread(r"D:\DIP\workshop\Sunglasses.png")

# Resize images
face = cv2.resize(face, (500, 600))
glass = cv2.resize(glass, (260, 90))

# Position of sunglasses
x = 120
y = 200

# Convert sunglass image to grayscale
gray = cv2.cvtColor(glass, cv2.COLOR_BGR2GRAY)

# Create mask
_, mask = cv2.threshold(gray, 240, 255, cv2.THRESH_BINARY_INV)

# Invert mask
mask_inv = cv2.bitwise_not(mask)

# Region of Interest (ROI)
roi = face[y:y+90, x:x+260]

# Remove background from ROI
bg = cv2.bitwise_and(roi, roi, mask=mask_inv)

# Extract sunglass foreground
fg = cv2.bitwise_and(glass, glass, mask=mask)

# Combine ROI and sunglass
combined = cv2.add(bg, fg)

# Place combined image back into face image
face[y:y+90, x:x+260] = combined

# Save output
cv2.imwrite("output.jpg", face)

# Convert BGR to RGB for display
face_rgb = cv2.cvtColor(face, cv2.COLOR_BGR2RGB)

# Display result
plt.imshow(face_rgb)
plt.axis("off")
plt.show()free to fork, contribute, or customize this project for your creative needs!


<img width="941" height="1672" alt="727ea275-2506-4c13-bdf2-a2f91672bcd5" src="https://github.com/user-attachments/assets/6eeea332-99be-460b-96a8-4a4e9d60d9ec" />

