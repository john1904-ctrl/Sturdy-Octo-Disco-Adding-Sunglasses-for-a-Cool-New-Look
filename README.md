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

## Program
```
import cv2
import numpy as np

# Read passport photo
img = cv2.imread("passport_photo.jpg")

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# Load Haar cascade classifiers
face_cascade = cv2.CascadeClassifier(
    cv2.data.haarcascades +
    "haarcascade_frontalface_default.xml"
)

eye_cascade = cv2.CascadeClassifier(
    cv2.data.haarcascades +
    "haarcascade_eye.xml"
)

# Detect face
faces = face_cascade.detectMultiScale(
    gray,
    1.1,
    5,
    minSize=(150, 150)
)

# Select the largest face
x, y, w, h = max(
    faces,
    key=lambda r: r[2] * r[3]
)

# Detect eyes inside the face
face_gray = gray[y:y+h, x:x+w]

eyes = eye_cascade.detectMultiScale(
    face_gray,
    1.1,
    6,
    minSize=(40, 40)
)

# Get two eye centers
eyes = sorted(
    eyes,
    key=lambda r: (r[1], r[0])
)[:2]

centers = [
    (x + ex + ew // 2, y + ey + eh // 2)
    for ex, ey, ew, eh in eyes
]

centers = sorted(centers)

left_eye, right_eye = centers[0], centers[1]

cx1, cy1 = left_eye
cx2, cy2 = right_eye

# Calculate sunglasses size from eye distance
eye_dist = cx2 - cx1

lens_w = int(eye_dist * 1.25)
lens_h = int(eye_dist * 0.62)

thickness = max(
    5,
    int(eye_dist * 0.045)
)

# Draw dark semi-transparent lenses
overlay = img.copy()

for cx, cy in [left_eye, right_eye]:

    cv2.ellipse(
        overlay,
        (
            cx,
            cy + int(eye_dist * 0.04)
        ),
        (
            lens_w // 2,
            lens_h // 2
        ),
        0,
        0,
        360,
        (35, 35, 35),
        -1,
        cv2.LINE_AA
    )

img = cv2.addWeighted(
    overlay,
    0.78,
    img,
    0.22,
    0
)

# Draw frames
for cx, cy in [left_eye, right_eye]:

    cv2.ellipse(
        img,
        (
            cx,
            cy + int(eye_dist * 0.04)
        ),
        (
            lens_w // 2,
            lens_h // 2
        ),
        0,
        0,
        360,
        (10, 10, 10),
        thickness,
        cv2.LINE_AA
    )

# Draw bridge
bridge_y = int(
    (cy1 + cy2) / 2 +
    eye_dist * 0.04
)

cv2.line(
    img,
    (
        cx1 + lens_w // 2 - thickness,
        bridge_y
    ),
    (
        cx2 - lens_w // 2 + thickness,
        bridge_y
    ),
    (10, 10, 10),
    thickness,
    cv2.LINE_AA
)

# Save result
cv2.imwrite(
    "sunglasses_output.jpg",
    img
)

# Display result
cv2.imshow(
    "Passport Photo with Sunglasses",
    img
)

cv2.waitKey(0)

cv2.destroyAllWindows()
```
## output
<img width="505" height="811" alt="image" src="https://github.com/user-attachments/assets/56df7f27-dc65-4e08-a134-509bbb8e5c8c" />

<img width="483" height="801" alt="image" src="https://github.com/user-attachments/assets/89a53941-f676-475a-9184-53a4b3e02a8a" />


Feel free to fork, contribute, or customize this project for your creative needs!
