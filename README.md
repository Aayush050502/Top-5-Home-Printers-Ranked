
# Printer Ranking and Visualization

## Overview

This project ranks printers based on various performance criteria such as scanning speed, cost per print, color quality, and overall speed. The project involves using a machine learning approach to rank these printers and visualize the results using 2D and 3D plots.

## Best Printers by Category

### 1. Best for Faster Scanning: HP Color LaserJet Pro MFP M283fdw
- **Features:**
  - Scanning Speed: Rapid scanning capabilities ideal for home offices.
  - All-in-One Functionality: Print, scan, copy, and fax.
  - Connectivity: Wi-Fi, Ethernet, and USB.
  - Resolution: 600 x 600 dpi (black and color).
- **Why It’s the Best:**
  - Known for its efficient scanning performance, the HP Color LaserJet Pro M283fdw offers quick and reliable scans, making it perfect for users who need to digitize documents swiftly.
- **Sources:**
  - TechGearLab Review
  - Amazon Listing

### 2. Best for Black and White Resolution: Xerox VersaLink B600DN
- **Features:**
  - Print Resolution: Up to 1200 x 1200 dpi (black).
  - Print Speed: Up to 58 ppm (pages per minute).
  - Paper Capacity: 700 sheets.
  - Connectivity: Ethernet and USB.
- **Why It’s the Best:**
  - The Xerox VersaLink B600DN excels in delivering crisp and clear black and white prints with high resolution, making it ideal for professional documents and text-heavy prints.
- **Sources:**
  - RTINGS.com Review
  - Amazon Listing

### 3. Best for Color Quality: Canon PIXMA Pro-200
- **Features:**
  - Print Resolution: Up to 4800 x 2400 dpi.
  - Ink System: 8-color dye-based ink for vibrant color reproduction.
  - Media Handling: Supports a wide range of paper types and sizes, including glossy and fine art papers.
  - Connectivity: Wi-Fi, Ethernet, and USB.
- **Why It’s the Best:**
  - The Canon PIXMA Pro-200 is renowned for its exceptional color accuracy and vibrant photo prints, making it a favorite among photographers and graphic designers.
- **Sources:**
  - The Spruce Reviews
  - Amazon Listing

### 4. Best for Cost per Print: Brother INKvestment MFC-J995DW
- **Features:**
  - Ink Efficiency: High-yield INKvestment cartridges reduce the cost per page.
  - Print Speed: Up to 12 ppm (black) and 10 ppm (color).
  - Connectivity: Wi-Fi, Ethernet, and USB.
  - Additional Features: All-in-one functionality with print, scan, copy, and fax.
- **Why It’s the Best:**
  - This Brother model offers an exceptionally low cost per print, thanks to its high-yield ink cartridges. It’s ideal for users who print frequently and want to minimize ongoing ink expenses.
- **Sources:**
  - RTINGS.com Review
  - Amazon Listing

### 5. Best for Speed: HP OfficeJet Pro 9025e
- **Features:**
  - Print Speed: Up to 24 ppm (black) and 20 ppm (color).
  - Scan Speed: Fast scanning with a flatbed scanner and automatic document feeder.
  - Connectivity: Wi-Fi, Ethernet, USB, and mobile printing options.
  - Additional Features: All-in-one functionality with print, scan, copy, and fax.
- **Why It’s the Best:**
  - The HP OfficeJet Pro 9025e is one of the fastest inkjet printers available for home use, making it perfect for users who need to handle large print jobs quickly without compromising on quality.
- **Sources:**
  - TechRadar Review
  - Amazon Listing

## Additional Resources and Articles
- **RTINGS.com Printer Reviews**: Comprehensive and in-depth reviews of various printers, including detailed performance metrics and comparisons.
- **TechRadar's Best Printers of 2024**: Expert reviews and recommendations for the latest printers on the market.
- **The Spruce’s Top Home Printers**: Curated lists and detailed articles on the best home printers for different needs and budgets.
- **Amazon.com Printer Listings**: Wide selection of printers with user reviews, ratings, and detailed product information to help you make an informed decision.

## Code Overview

This project involves the following steps:

### Step 1: Load the CSV File
- Upload your CSV file containing printer data. This file should include information on various printers and their features.

### Step 2: Define and Normalize Criteria
- Define the criteria (e.g., Faster Scan, Cost per Print, Colour Quality, Speed).
- Normalize the criteria scores to ensure they are on the same scale.

### Step 3: Assign Weights and Rank Printers
- Assign weights to each criterion based on their importance.
- Calculate a weighted score for each printer and rank them accordingly.

### Step 4: Visualize the Rankings
- **2D Visualization**: Create bar plots to visualize the weighted scores and ranks.
- **3D Visualization**: Create a 3D bar graph to visualize the rankings based on weighted scores.

### Sample Code

```python
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from google.colab import files  # If using Google Colab

# Step 1: Upload CSV File
# Uncomment the below two lines if you are using Google Colab to upload the file
# uploaded = files.upload() 
# printers_df = pd.read_csv(list(uploaded.keys())[0])

# For a local environment, replace with:
printers_df = pd.read_csv('your_file.csv')  # Replace 'your_file.csv' with your actual file path

# Step 2: Define the criteria and their mock scores
criteria = {
    "Faster Scan": [9, 6, 7, 8, 8],  # Modify these based on your dataset
    "Cost per Print": [6, 5, 8, 9, 7],
    "Colour Quality": [8, 7, 9, 7, 6],
    "Speed": [7, 8, 7, 6, 9]
}

# Convert criteria to DataFrame
criteria_df = pd.DataFrame(criteria)

# Normalize the scores
scaler = MinMaxScaler()
criteria_scaled = scaler.fit_transform(criteria_df)

# Convert back to DataFrame
criteria_scaled_df = pd.DataFrame(criteria_scaled, columns=criteria_df.columns)

# Assign weights to each criterion
weights = {
    "Faster Scan": 0.25,
    "Cost per Print": 0.25,
    "Colour Quality": 0.25,
    "Speed": 0.25
}

# Calculate the weighted score for each printer
criteria_scaled_df['Weighted Score'] = np.dot(criteria_scaled_df, list(weights.values()))

# Add the weighted score to the original DataFrame
printers_df['Weighted Score'] = criteria_scaled_df['Weighted Score']

# Rank the printers based on the weighted score
printers_df['Rank'] = printers_df['Weighted Score'].rank(ascending=False)

# Step 3: Visualize the Rankings
# Bar plot for Weighted Scores
plt.figure(figsize=(10, 6))
sns.barplot(x='Weighted Score', y='Printer Model', data=printers_df.sort_values('Rank', ascending=True), palette='viridis')
plt.title('Printer Ranking Based on Weighted Scores')
plt.xlabel('Weighted Score')
plt.ylabel('Printer Model')
plt.show()

# Bar plot for Rank
plt.figure(figsize=(10, 6))
sns.barplot(x='Rank', y='Printer Model', data=printers_df.sort_values('Rank', ascending=True), palette='viridis')
plt.title('Printer Ranking')
plt.xlabel('Rank')
plt.ylabel('Printer Model')
plt.show()

# Display the final rankings table
print(printers_df[['Printer Model', 'Weighted Score', 'Rank']])
```

## How to Use
1. **Upload your CSV file**: Ensure your file contains relevant printer data.
2. **Run the code**: Execute the provided Python script to rank the printers and generate visualizations.
3. **Analyze the results**: Use the visualizations and rankings to make informed decisions on which printer best meets your needs.

## License
This project is licensed under the MIT License.
