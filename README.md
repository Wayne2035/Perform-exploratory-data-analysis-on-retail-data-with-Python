import pandas as pd
df = pd.read_excel("Online Retail.xlsx")
print(df.head(10))
print(df.isnull().sum())
df = df.dropna()
print(df.info())
df = df.drop_duplicates()
print(df.info())
df.to_csv('cleaned Online Retail.xlsx', index=False)
import pandas as pd
df = pd.read_csv('cleaned Online Retail.xlsx')
df.head()
df.describe()
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('cleaned Online Retail.xlsx')
plt.figure(figsize=(8,5))
sns.histplot(df['Quantity'], bins=50, kde=True)
plt.title('Distribution of Quantity Purchased')
plt.show()
top_products = df.groupby('Description')['Quantity'].sum().sort_values(ascending=False).head(10)
plt.figure(figsize=(10,5))
top_products.plot(kind='bar', color='royalblue')
plt.title('Top 10 Best Selling Products')
plt.ylabel('Total Quantity Sold')
plt.xticks(rotation=45)
plt.show()
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate']) 
df.set_index('InvoiceDate', inplace=True)
df.resample('M')['Quantity'].sum().plot(figsize=(12,6), color='green')
plt.title('Sales Trend Over Time')
plt.ylabel('Total Quantity Sold')
plt.show()
plt.figure(figsize=(8,6))
sns.scatterplot(x=df['UnitPrice'], y=df['Quantity'], alpha=0.5)
plt.title('Price vs Quantity Purchased')
plt.xlabel('Unit Price')
plt.ylabel('Quantity Sold')
plt.show()
import pandas as pd
import matplotlib.pyplot as plt

file_path = "Online Retail.xlsx"
df = pd.read_excel(file_path)
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])

df['Month'] = df['InvoiceDate'].dt.month
df['DayOfWeek'] = df['InvoiceDate'].dt.day_name()

df['TotalSales'] = df['Quantity'] * df['UnitPrice']

monthly_sales = df.groupby('Month')['TotalSales'].sum()

weekday_sales = df.groupby('DayOfWeek')['TotalSales'].sum()

order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
weekday_sales = weekday_sales.reindex(order)
plt.figure(figsize=(12,5))

# Monthly 
plt.subplot(1,2,1)
monthly_sales.plot(kind='line', marker='o', color='b')
plt.title("Total Sales by Month")
plt.xlabel("Month")
plt.ylabel("Total Sales")
plt.grid(True)

# Weekday sales
plt.subplot(1,2,2)
weekday_sales.plot(kind='bar', color='g')
plt.title("Total Sales by Day of the Week")
plt.xlabel("Day of the Week")
plt.ylabel("Total Sales")
plt.xticks(rotation=45)
plt.grid(axis='y')

plt.tight_layout()
plt.show()
file_path = "Online Retail.xlsx"
df = pd.read_excel(file_path)

# Group by product description and sum the quantity sold
top_products = df.groupby('Description')['Quantity'].sum().sort_values(ascending=False).head(10)

# Group by country and sum the quantity sold
top_countries = df.groupby('Country')['Quantity'].sum().sort_values(ascending=False).head(10)

# Plot top-selling products
plt.figure(figsize=(12,5))

plt.subplot(1,2,1)
top_products.plot(kind='bar', color='c')
plt.title("Top 10 Best-Selling Products")
plt.xlabel("Product Description")
plt.ylabel("Total Quantity Sold")
plt.xticks(rotation=90)
plt.grid(axis='y')

# Plot top-selling countries
plt.subplot(1,2,2)
top_countries.plot(kind='bar', color='m')
plt.title("Top 10 Best-Selling Countries")
plt.xlabel("Country")
plt.ylabel("Total Quantity Sold")
plt.xticks(rotation=45)
plt.grid(axis='y')

plt.tight_layout()
plt.show()
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
file_path = "Online Retail.xlsx"
df = pd.read_excel(file_path)

# Check for negative or zero quantities (potential returns/errors)
negative_quantities = df[df['Quantity'] <= 0]

# Check for unusually high quantities (outliers)
high_quantity_threshold = df['Quantity'].quantile(0.99)  # 99th percentile
high_quantity_orders = df[df['Quantity'] > high_quantity_threshold]

# Check for zero or negative prices (potential data entry errors)
zero_prices = df[df['UnitPrice'] <= 0]

# Identify duplicate invoices
duplicate_invoices = df[df.duplicated(subset=['InvoiceNo', 'StockCode', 'Quantity', 'UnitPrice'], keep=False)]

# Check for missing values
missing_values = df.isnull().sum()

# Visualization: Boxplot for quantity and unit price
plt.figure(figsize=(12,5))

plt.subplot(1,2,1)
sns.boxplot(x=df['Quantity'])
plt.title("Boxplot of Quantity Sold")

plt.subplot(1,2,2)
sns.boxplot(x=df['UnitPrice'])
plt.title("Boxplot of Unit Price")

plt.tight_layout()
plt.show()

# Print key findings
print("Negative or Zero Quantity Orders:\n", negative_quantities.head())
print("\nHigh Quantity Orders (99th Percentile):\n", high_quantity_orders.head())
print("\nZero or Negative Price Transactions:\n", zero_prices.head())
print("\nDuplicate Invoices:\n", duplicate_invoices.head())
print("\nMissing Values:\n", missing_values)
