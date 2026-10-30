# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 30-10-2026
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
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load the BMW car sales dataset
data = pd.read_csv('/content/BMW_Car_Sales_Classification.csv')

print("Original Data:")
print(data.head())

# Aggregate sales volume by year
resampled_data = (
    data.groupby('Year')['Sales_Volume']
    .sum()
    .reset_index()
)

print("\nYearly Sales Volume:")
print(resampled_data.head())

# Extract years and sales volume
years = resampled_data['Year'].tolist()
sales = resampled_data['Sales_Volume'].tolist()

# Create centered X values
X = [year - years[len(years) // 2] for year in years]

# Linear Trend Estimation
x2 = [x**2 for x in X]
xy = [x * y for x, y in zip(X, sales)]

n = len(years)

if n < 3:
    raise ValueError("At least 3 distinct years are needed for a degree-2 polynomial trend.")

denominator = n * sum(x2) - sum(X)**2

if denominator == 0:
    raise ValueError("Cannot calculate the linear trend with these year values.")

b = (n * sum(xy) - sum(sales) * sum(X)) / denominator
a = (sum(sales) - b * sum(X)) / n

linear_trend = [a + b * x for x in X]

# Polynomial Trend Estimation (Degree 2)
x3 = [x**3 for x in X]
x4 = [x**4 for x in X]
x2y = [x2[i] * sales[i] for i in range(n)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(sales), sum(xy), sum(x2y)]

solution = np.linalg.solve(np.array(coeff, dtype=float),
                           np.array(Y, dtype=float))

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * x + c_poly * x**2
    for x in X
]

# Display trend equations
print(f"\nLinear Trend: y = {a:.2f} + {b:.2f}x")
print(
    f"Polynomial Trend: y = {a_poly:.2f} "
    f"+ {b_poly:.2f}x + {c_poly:.2f}x²"
)

# Add trends to the dataframe
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

# Visualize the results
plt.figure(figsize=(12, 6))

plt.plot(
    years, sales,
    color='blue', marker='o',
    label='Actual Sales Volume'
)

plt.plot(
    years, linear_trend,
    color='black', linestyle='--',
    label='Linear Trend'
)

plt.plot(
    years, poly_trend,
    color='red', marker='o',
    label='Polynomial Trend (Degree 2)'
)

plt.title('BMW Car Sales: Linear and Polynomial Trend')
plt.xlabel('Year')
plt.ylabel('Total Sales Volume')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# Display final results
print("\nFinal Results:")
print(resampled_data)
```

### OUTPUT

<img width="1083" height="522" alt="image" src="https://github.com/user-attachments/assets/5592b00c-74df-4f2a-96bb-025a0342fa10" />

<img width="1422" height="665" alt="image" src="https://github.com/user-attachments/assets/98ad9687-fe4b-4117-8aa5-8e52e422207e" />

<img width="587" height="377" alt="image" src="https://github.com/user-attachments/assets/d08b583c-da39-4374-b2f7-ca00e958bd64" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
