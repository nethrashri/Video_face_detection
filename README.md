import cv2
import numpy as np
import matplotlib.pyplot as plt

def find_house_perimeter(image_path):
    # Step 1: Read the floor plan image
    image = cv2.imread(image_path)
    
    # Step 2: Convert to grayscale
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    
    # Step 3: Apply a binary threshold to segment the house structure
    _, thresholded = cv2.threshold(gray, 200, 255, cv2.THRESH_BINARY_INV)
    
    # Step 4: Find contours in the thresholded image
    contours, _ = cv2.findContours(thresholded, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
    # Assuming the largest contour is the house perimeter
    house_contour = max(contours, key=cv2.contourArea)
    
    # Step 5: Calculate the perimeter (arc length) of the house contour
    perimeter = cv2.arcLength(house_contour, True)
    
    # Step 6: Visualize the contour on the image
    output_image = image.copy()
    cv2.drawContours(output_image, [house_contour], -1, (0, 255, 0), 2)
    
    # Display the original image and the contour-drawn image
    plt.figure(figsize=(10, 5))
    plt.subplot(1, 2, 1)
    plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
    plt.title("Original Image")
    
    plt.subplot(1, 2, 2)
    plt.imshow(cv2.cvtColor(output_image, cv2.COLOR_BGR2RGB))
    plt.title("Detected Perimeter")
    
    plt.show()
    
    return perimeter

# Provide the path to your floor plan image
image_path = 'housemap.png'
perimeter = find_house_perimeter(image_path)
print(f"Perimeter of the house: {perimeter} pixels")
