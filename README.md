# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:
25/04/2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
## Murali Krishna S
## 212223230129

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# -----------------------------
# Load Data
# -----------------------------
data = pd.read_excel('/content/Sales_v1.xlsx')

# Clean column names (IMPORTANT FIX)
data.columns = data.columns.str.strip()

# Fix column name if space exists
if 'Sales' not in data.columns:
    for col in data.columns:
        if col.lower().strip() == 'sales':
            data.rename(columns={col: 'Sales'}, inplace=True)

# -----------------------------
# Create Time Index (Year + Month)
# -----------------------------
# Ensure sorting
data = data.sort_values(by=['Year', 'Month Number'])

# Create continuous time index
data['TimeIndex'] = range(len(data))

# -----------------------------
# Aggregate Data (Monthly Trend)
# -----------------------------
resampled_data = data.groupby('TimeIndex')['Sales'].sum().reset_index()

# Extract values
X = resampled_data['TimeIndex'].values
Y = resampled_data['Sales'].values

n = len(X)

# -----------------------------
# Linear Trend (using numpy)
# -----------------------------
linear_coeffs = np.polyfit(X, Y, 1)
linear_trend = np.polyval(linear_coeffs, X)

# -----------------------------
# Polynomial Trend (SAFE)
# -----------------------------
if n >= 3:
    poly_degree = 2
else:
    poly_degree = max(1, n - 1)

poly_coeffs = np.polyfit(X, Y, poly_degree)
poly_trend = np.polyval(poly_coeffs, X)

# -----------------------------
# Print Equations
# -----------------------------
print("\nLinear Trend Equation:")
print(f"y = {linear_coeffs[0]:.2f}x + {linear_coeffs[1]:.2f}")

if poly_degree == 2:
    print("\nPolynomial Trend Equation (Quadratic):")
    print(f"y = {poly_coeffs[0]:.2f}x² + {poly_coeffs[1]:.2f}x + {poly_coeffs[2]:.2f}")
elif poly_degree == 1:
    print("\nPolynomial Trend (Reduced to Linear):")
    print(f"y = {poly_coeffs[0]:.2f}x + {poly_coeffs[1]:.2f}")
else:
    print("\nPolynomial Trend (Constant):")
    print(f"y = {poly_coeffs[0]:.2f}")

# -----------------------------
# Plot
# -----------------------------
plt.figure(figsize=(12,6))

plt.plot(X, Y, marker='o', label='Actual Sales')
plt.plot(X, linear_trend, linestyle='--', label='Linear Trend')
plt.plot(X, poly_trend, color='green', label='Polynomial Trend')

plt.xlabel('Time Index (Monthly)')
plt.ylabel('Sales')
plt.title('Sales Trend Analysis (Improved)')
plt.legend()
plt.grid()

plt.show()
```
### OUTPUT
<img width="1014" height="648" alt="image" src="https://github.com/user-attachments/assets/0c971acf-0ea7-44c1-9434-5645697cdb31" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
