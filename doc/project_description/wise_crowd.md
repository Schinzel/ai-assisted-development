# WiseCrowd Platform

## Overview
WiseCrowd is an investment advisory platform that leverages collective intelligence to provide optimized investment recommendations. 
Instead of relying on traditional financial advisors who may have conflicting interests, WiseCrowd analyzes the behavior and 
performance of many investors to identify the most successful strategies.

## Key Problems Addressed
- **Investment Complexity**: Many people find investments difficult to understand and manage effectively
- **Misaligned Interests**: Financial institutions often prioritize their own products (e.g., Swedbank recommending Robur funds)
- **High Advisory Costs**: Traditional financial advice is expensive, especially with regulatory requirements like the MiFID directive

## Solution Approach
WiseCrowd's methodology follows these steps:

1. **Top Investor Identification**: Analyze a large pool of investors to identify the top-performing ~5% based on metrics like Sharpe ratio, total returns, and consistency
2. **Asset Allocation Analysis**: Study these top investors' portfolios to determine optimal asset class allocation according to Modern Portfolio Theory
3. **Fund Selection**: Within each asset class, identify the most popular and successful fund choices
4. **Recommendation Generation**: Provide users with personalized recommendations based on this "wisdom of the crowd" approach

## Technical Implementation
The platform uses Kotlin for its backend implementation and leverages data services like Morningstar to obtain fund information including prices, Sharpe ratios, and asset classifications.

## Data Requirements
To develop and test the WiseCrowd platform effectively, realistic data sets are needed that can simulate:
- Various investor profiles and behaviors
- Historical transaction patterns
- Asset performance across different market conditions
- Different investment strategies and their outcomes

This is where the WiseCrowd Data Generator comes in - providing synthetic data that enables development and testing of the platform's core algorithms and performance at scale.
