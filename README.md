import pandas as pd

# Load the CSV
df = pd.read_csv("file1.csv")

# Drop unnecessary columns
df.drop(columns=["Unnamed: 0"], inplace=True)

# Strip whitespace and fix case in categorical columns
df['Gender'] = df['Gender'].str.strip().str.lower().str.capitalize()
df['MaritalStatus'] = df['MaritalStatus'].str.strip().str.upper()
df['Education'] = df['Education'].str.strip().str.capitalize()
df['HealthCondition'] = df['HealthCondition'].str.strip().str.capitalize()
df['ShowType'] = df['ShowType'].str.strip().str.lower().str.capitalize()
df['ShowGenre'] = df['ShowGenre'].str.strip().str.capitalize()
df['ProductCategory'] = df['ProductCategory'].str.strip().str.capitalize()

# Convert dates to datetime
df['AppointmentDay'] = pd.to_datetime(df['AppointmentDay'], errors='coerce')
df['PurchaseDate'] = pd.to_datetime(df['PurchaseDate'], errors='coerce')

# Convert 'NoShow' to Boolean
df['NoShow'] = df['NoShow'].map({'Yes': True, 'No': False})

# Handle missing values
# Fill categorical NaNs with 'Unknown'
categorical_cols = ['Education', 'HealthCondition', 'ShowGenre', 'ProductCategory']
df[categorical_cols] = df[categorical_cols].fillna('Unknown')

# Fill numerical NaNs with median
df['AnnualIncome'] = df['AnnualIncome'].fillna(df['AnnualIncome'].median())

# Drop rows with any remaining critical missing values (if any)
df.dropna(subset=['CustomerID', 'PurchaseAmount'], inplace=True)

# Convert numeric fields to correct types
df['NetflixRating'] = pd.to_numeric(df['NetflixRating'], errors='coerce')
df['PurchaseAmount'] = pd.to_numeric(df['PurchaseAmount'], errors='coerce')

# Final cleanup: reset index
df.reset_index(drop=True, inplace=True)

# Preview cleaned data
df.head(50)
