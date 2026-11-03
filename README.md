#  EXP - 7 HOUGH TRANSFORM

##  Aim:

To implement a basic lane detection pipeline using OpenCV by completing missing code segments at specified locations.

---

## Learning Objective:

* Understand each stage of image processing
* Learn how to build a complete computer vision pipeline
* Practice writing code in guided sections

**Important Instruction:**
👉 Write code **ONLY in places marked as `# Your Code Here`**
👉 Do NOT modify any other part of the code

---

##  Software Used:

* Anaconda – Python 3.7
* Jupyter Notebook / VS Code
* OpenCV (cv2)
* NumPy
* Matplotlib

---
##  Developed By:

* **Name:** R P Loshini
* **Register No:** 212225230155
---

##  Algorithm & Explanation:
#### Step 1: Import Libraries
Import the required modules: OpenCV (cv2), NumPy (numpy), and Matplotlib (matplotlib.pyplot).

#### Step 2: Read the Image
Load the input image from the local directory (lane.jpg or repository image) using cv2.imread() and convert it to RGB color space for Matplotlib rendering.

#### Step 3: Convert to Grayscale
Convert the original BGR image into a single-channel grayscale image using cv2.cvtColor().

#### Step 4: Display Images
Display the original RGB image and the grayscale image side-by-side using Matplotlib.

#### Step 5: Thresholding
Apply binary thresholding using cv2.threshold() to isolate bright pixels (potential lane markings) from the background.

#### Step 6: Region of Interest (ROI) Masking
Create a polygon mask using np.zeros_like() and cv2.fillPoly() to isolate the road region containing the lane markings, discarding irrelevant areas.

#### Step 7: Edge Detection
Apply the Canny edge detector using cv2.Canny() to detect high-intensity gradients corresponding to lane boundaries.

#### Step 8: Gaussian Blur
Apply a Gaussian smoothing filter using cv2.GaussianBlur() to reduce high-frequency noise and smooth out detected edges.

#### Step 9: Hough Transform
Detect straight lines in the blurred binary edge image using cv2.HoughLinesP().

#### Step 10: Lane Detection Logic & Overlay
Draw the detected lane lines onto a blank image canvas using cv2.line() and merge them with the original image using cv2.addWeighted() to produce the final output.

---

###  Step 1: Import Libraries

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

---

###  Step 2: Read the Image

```python
image_path = r"C:\Users\acer\DIPT\lane.jpg"
image_bgr = cv2.imread(image_path)
if image_bgr is None:
    # Fallback to local repo image if lane.jpg is not present
    image_path = r"C:\Users\acer\DIPT\Apollo-11-launch.jpg"
    image_bgr = cv2.imread(image_path)

image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)
```

---

###  Step 3: Convert to Grayscale

```python
gray_image = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)
```

---

###  Step 4: Display Images

```python
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(image_rgb)
plt.title("Original Image")
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(gray_image, cmap='gray')
plt.title("Grayscale Image")
plt.axis('off')

plt.show()
```

---

###  Step 5: Thresholding

```python
_, threshold_image = cv2.threshold(gray_image, 200, 255, cv2.THRESH_BINARY)
```

---

###  Step 6: Region of Interest (ROI)

```python
height, width = gray_image.shape
mask = np.zeros_like(gray_image)
polygons = np.array([
    [(0, height), (width, height), (int(width * 0.55), int(height * 0.6)), (int(width * 0.45), int(height * 0.6))]
])
cv2.fillPoly(mask, polygons, 255)
roi_image = cv2.bitwise_and(threshold_image, mask)

# Display Thresholded and ROI Masked Images
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(threshold_image, cmap='gray')
plt.title("Thresholded Image")
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(roi_image, cmap='gray')
plt.title("ROI Masked Image")
plt.axis('off')

plt.show()
```

---

### Step 7: Edge Detection (Canny)

```python
canny_edges = cv2.Canny(gray_image, 50, 150)
```

---

###  Step 8: Gaussian Blur

```python
blurred_image = cv2.GaussianBlur(canny_edges, (5, 5), 0)

# Display Canny Edges and Smoothed Image
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(canny_edges, cmap='gray')
plt.title("Edge Detected Image (Canny)")
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(blurred_image, cmap='gray')
plt.title("Smoothed Image (Gaussian Blur)")
plt.axis('off')

plt.show()
```

---

###  Step 9: Hough Transform

```python
lines = cv2.HoughLinesP(blurred_image, rho=2, theta=np.pi/180, threshold=100, minLineLength=40, maxLineGap=5)
```

---

### Step 10: Lane Detection Logic

```python
line_image = np.zeros_like(image_rgb)
if lines is not None:
    for line in lines:
        # Robust unpacking handling both 2D and 3D output shapes
        points = line[0] if len(line.shape) > 1 else line
        x1, y1, x2, y2 = map(int, points)
        cv2.line(line_image, (x1, y1), (x2, y2), (255, 0, 0), 10)

final_output = cv2.addWeighted(image_rgb, 0.8, line_image, 1.0, 0)

# Display Detected Lines and Final Pipeline Output
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(line_image)
plt.title("Detected Lines")
plt.axis('off')

plt.subplot(1, 2, 2)
plt.imshow(final_output)
plt.title("Final Lane Detection Output")
plt.axis('off')

plt.show()
```

---

## Output:
<img width="787" height="225" alt="image" src="https://github.com/user-attachments/assets/fea5d44a-7e15-49dc-8438-f84f7e6b123d" />
<img width="787" height="231" alt="image" src="https://github.com/user-attachments/assets/1021ae1e-6adc-48ec-9f49-047a6304909c" />
<img width="788" height="233" alt="image" src="https://github.com/user-attachments/assets/a0d9712c-c5fc-480e-b363-8857e087cf33" />
<img width="782" height="226" alt="image" src="https://github.com/user-attachments/assets/2bdee6b3-ad7c-4d05-870c-69932536b3d5" />


---


## Result

Thus, the lane detection pipeline is successfully implemented by completing the missing code sections. The system detects and highlights lane lines effectively.

---


