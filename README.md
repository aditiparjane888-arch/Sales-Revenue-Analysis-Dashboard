# Sales-Revenue-Analysis-Dashboard
import streamlit as st
import pandas as pd
import plotly.express as px

st.title("Sales & Revenue Analysis Dashboard")

# Upload CSV file
file = st.file_uploader("Upload CSV File", type=["csv"])

if file:
    df = pd.read_csv(file)

    st.subheader("Dataset")
    st.write(df.head())

    # KPIs
    total_sales = df["Sales"].sum()
    total_revenue = df["Revenue"].sum()

    col1, col2 = st.columns(2)
    col1.metric("Total Sales", total_sales)
    col2.metric("Total Revenue", f"₹{total_revenue:,.2f}")

    # Revenue Trend
    fig1 = px.line(df, x="Date", y="Revenue",
                   title="Revenue Trend")
    st.plotly_chart(fig1)

    # Top Products
    product_sales = df.groupby("Product")["Sales"].sum().reset_index()

    fig2 = px.bar(product_sales,
                  x="Product",
                  y="Sales",
                  title="Top Performing Products")
    st.plotly_chart(fig2)

    # Filter
    products = st.multiselect(
        "Select Product",
        df["Product"].unique()
    )

    if products:
        filtered_df = df[df["Product"].isin(products)]
        st.write(filtered_df)