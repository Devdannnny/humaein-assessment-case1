# Case Study #1: Claim Resubmission Ingestion Pipeline - Humaein Assessment

## 🎯 Project Overview

**Author:** Daniel Dabiri  
**Date Completed:** 19th Aug 2025  
**Type:** Insurance Claims Processing Pipeline  
**Technology Stack:** Python, Pandas, JSON/CSV Processing, Data Normalization

## 📋 Executive Summary

This project implements a robust, production-ready pipeline for ingesting, normalizing, and analyzing insurance claim data from multiple Electronic Medical Record (EMR) systems. The pipeline identifies claims eligible for resubmission based on business rules, denial reasons, and temporal criteria.

## 🚀 Quick Start Guide

### **Prerequisites**

1. **Python 3.8+** installed on your system
2. **Jupyter Notebook** (optional, for interactive development)
3. **Git** (for cloning the repository)

### **Installation Steps**

1. **Clone or download the project:**

   ```bash
   git clone https://github.com/Devdannnny/humaein-assessment-case1
   cd case1
   ```

2. **Install required dependencies:**

   ```bash
   pip install pandas numpy python-dateutil
   ```

3. **Verify installation:**
   ```bash
   python -c "import pandas, json, csv; print('✅ All dependencies installed successfully!')"
   ```

## 🏃‍♂️ How to Run the Codebase

### **Method 1: Jupyter Notebook (Recommended for Development)**

1. **Start Jupyter Notebook:**

   ```bash
   jupyter notebook
   ```

2. **Open the notebook:**

   - Navigate to `Case 1.ipynb` in the Jupyter interface
   - Click on the file to open it

3. **Run the cells:**

   - **Cell 1:** Installs pandas (run once)
   - **Cells 2-17:** Define all the classes and functions
   - **Cell 18:** Main execution (creates sample data and runs pipeline)
   - **Cell 19:** Displays results

4. **Expected output:**

   ```
   Sample data files created: emr_alpha.csv and emr_beta.json
   [Pipeline execution logs...]

   ============================================================
   PIPELINE METRICS
   ============================================================

   📊 Data Sources:
     - EMR Alpha: 5 claims
     - EMR Beta: 4 claims

   📈 Processing Summary:
     - Total claims processed: 9
     - Denied claims: 7
     - Eligible for resubmission: 4

   ❌ Exclusion Reasons:
     - Missing patient ID: 2
     - Too recent (≤7 days): 0
     - Non-retryable denial reason: 1

   ✅ Resubmission Rate: 44.4%
   ```

### **Method 2: Python Script (Production Use)**

1. **Create a Python script** (e.g., `run_pipeline.py`):

   ```python
   #!/usr/bin/env python3

   # Import the pipeline classes
   from Case_1 import ClaimResubmissionPipeline, create_sample_data

   def main():
       # Create sample data files
       create_sample_data()

       # Initialize and run the pipeline
       pipeline = ClaimResubmissionPipeline()

       # Run with sample data
       pipeline.run(
           alpha_file='emr_alpha.csv',
           beta_file='emr_beta.json',
           output_file='resubmission_candidates.json'
       )

       print("✅ Pipeline execution completed!")
       print("📁 Check the output files for results.")

   if __name__ == "__main__":
       main()
   ```

2. **Run the script:**
   ```bash
   python run_pipeline.py
   ```

### **Method 3: Interactive Python Session**

1. **Start Python interpreter:**

   ```bash
   python
   ```

2. **Run the pipeline interactively:**

   ```python
   >>> # Import required modules
   >>> import json
   >>> import csv
   >>> import logging
   >>> from datetime import datetime, timedelta
   >>> from typing import Dict, List, Optional, Tuple, Any
   >>> from dataclasses import dataclass, asdict
   >>> from enum import Enum
   >>> import pandas as pd
   >>> from pathlib import Path
   >>> import sys
   >>> from abc import ABC, abstractmethod

   >>> # [Copy and paste all the class definitions from the notebook]
   >>> # ... (all the classes: Config, ClaimStatus, UnifiedClaim, etc.)

   >>> # Create sample data and run pipeline
   >>> create_sample_data()
   >>> pipeline = ClaimResubmissionPipeline()
   >>> pipeline.run('emr_alpha.csv', 'emr_beta.json', 'resubmission_candidates.json')
   ```

## 📁 Understanding the File Structure

```
case1/
├── Case 1.ipynb                 # Main Jupyter notebook with implementation
├── README.md                    # This documentation
├── emr_alpha.csv               # Sample EMR Alpha data (CSV format)
├── emr_beta.json               # Sample EMR Beta data (JSON format)
├── resubmission_candidates.json # Pipeline output (eligible claims)
├── rejected_claims.json        # Non-eligible denied claims
├── pipeline_metrics.json       # Processing statistics
└── claim_pipeline.log          # Detailed execution logs
```

### **Input Data Formats**

**EMR Alpha (CSV):**

```csv
claim_id,patient_id,procedure_code,denial_reason,submitted_at,status
A123,P001,99213,Missing modifier,2025-07-01,denied
A124,P002,99214,Incorrect NPI,2025-07-10,denied
```

**EMR Beta (JSON):**

```json
[
  {
    "id": "B987",
    "member": "P010",
    "code": "99213",
    "error_msg": "Incorrect provider type",
    "date": "2025-07-03T00:00:00",
    "status": "denied"
  }
]
```

### **Output Files**

**resubmission_candidates.json:**

```json
[
  {
    "claim_id": "A123",
    "resubmission_reason": "missing modifier",
    "source_system": "alpha",
    "recommended_changes": "Add appropriate modifier codes to the claim"
  }
]
```

**pipeline_metrics.json:**

```json
{
  "source_counts": { "alpha": 5, "beta": 4 },
  "total_claims": 9,
  "eligible_claims": 4,
  "processing_errors": 0
}
```

## 🔧 Configuration Options

### **Business Rules Configuration**

You can modify the business rules in the `Config` class:

```python
class Config:
    # Change the reference date for eligibility calculation
    REFERENCE_DATE = datetime(2025, 7, 30)

    # Modify the minimum age threshold for claims
    DAYS_THRESHOLD = 7  # Claims must be older than 7 days

    # Add or remove retryable denial reasons
    RETRYABLE_REASONS = {
        "missing modifier",
        "incorrect npi",
        "prior auth required",
        "your_new_reason"  # Add custom reasons here
    }

    # Add or remove non-retryable denial reasons
    NON_RETRYABLE_REASONS = {
        "authorization expired",
        "incorrect provider type",
        "your_non_retryable_reason"  # Add custom reasons here
    }
```

### **Logging Configuration**

Modify logging behavior:

```python
# Configure logging level (DEBUG, INFO, WARNING, ERROR)
logging.basicConfig(
    level=logging.INFO,  # Change to logging.DEBUG for more details
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('claim_pipeline.log'),
        logging.StreamHandler(sys.stdout)
    ]
)
```

## 🧪 Testing with Your Own Data

### **Using Your Own EMR Alpha Data (CSV)**

1. **Prepare your CSV file** with the required columns:

   ```csv
   claim_id,patient_id,procedure_code,denial_reason,submitted_at,status
   YOUR001,PAT001,99213,Missing modifier,2025-07-01,denied
   YOUR002,PAT002,99214,Incorrect NPI,2025-07-10,denied
   ```

2. **Run the pipeline with your data:**
   ```python
   pipeline = ClaimResubmissionPipeline()
   pipeline.run(
       alpha_file='your_emr_alpha.csv',
       beta_file='emr_beta.json',  # or your own beta file
       output_file='your_results.json'
   )
   ```

### **Using Your Own EMR Beta Data (JSON)**

1. **Prepare your JSON file** with the required structure:

   ```json
   [
     {
       "id": "UNIQUE_ID",
       "member": "PAT001",
       "code": "99213",
       "error_msg": "Missing modifier",
       "date": "2025-07-01T00:00:00",
       "status": "denied"
     }
   ]
   ```

2. **Run the pipeline with your data:**
   ```python
   pipeline = ClaimResubmissionPipeline()
   pipeline.run(
       alpha_file='emr_alpha.csv',  # or your own alpha file
       beta_file='your_emr_beta.json',
       output_file='your_results.json'
   )
   ```

## 🔍 Troubleshooting

### **Common Issues and Solutions**

1. **Import Error: No module named 'pandas'**

   ```bash
   pip install pandas
   ```

2. **File Not Found Error**

   - Ensure you're in the correct directory (`case1/`)
   - Check that input files exist (`emr_alpha.csv`, `emr_beta.json`)

3. **Permission Error when writing output files**

   - Ensure you have write permissions in the current directory
   - Check if files are open in another application

4. **Date Parsing Errors**

   - Ensure dates are in the correct format (YYYY-MM-DD or ISO format)
   - Check for invalid date values in your data

5. **Memory Issues with Large Files**
   - The pipeline is optimized for large datasets
   - If you encounter memory issues, consider processing in batches

### **Debug Mode**

Enable debug logging for detailed troubleshooting:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## 📊 Understanding the Results

### **Pipeline Metrics Explained**

- **Total Claims**: Number of claims processed from all sources
- **Denied Claims**: Claims with status "denied"
- **Eligible Claims**: Claims that meet all resubmission criteria
- **Resubmission Rate**: Percentage of total claims eligible for resubmission

### **Exclusion Reasons**

- **Missing Patient ID**: Claims without a valid patient identifier
- **Too Recent**: Claims submitted within the last 7 days (configurable)
- **Non-Retryable Denial Reason**: Claims with denial reasons that cannot be fixed

### **Business Logic**

The pipeline applies these criteria to determine eligibility:

1. **Status Check**: Claim must be "denied"
2. **Patient ID Check**: Must have a valid patient identifier
3. **Age Check**: Must be older than 7 days (configurable)
4. **Denial Reason Check**: Must have a retryable denial reason

## 🏗️ Architecture & Design

### **Core Components**

1. **Data Ingestion Layer**

   - Multi-format support (CSV, JSON)
   - EMR system-specific adapters
   - Error handling and validation

2. **Data Normalization Engine**

   - Unified data model across systems
   - Field mapping and transformation
   - Data quality validation

3. **Business Logic Engine**

   - Denial reason classification
   - Resubmission eligibility rules
   - Temporal validation

4. **Output Generation**
   - Structured JSON output
   - Comprehensive logging
   - Performance metrics

### **Design Patterns Used**

- **Strategy Pattern**: Different EMR adapters
- **Factory Pattern**: Data source creation
- **Pipeline Pattern**: Sequential processing stages
- **Observer Pattern**: Logging and metrics collection

## 🔧 Technical Implementation

### **Key Classes**

- **`ClaimResubmissionPipeline`**: Main orchestrator
- **`EMRAlphaHandler`**: CSV data processing
- **`EMRBetaHandler`**: JSON data processing
- **`DenialReasonClassifier`**: Business logic for denial analysis
- **`ResubmissionEligibilityEngine`**: Eligibility determination

### **Data Model**

```python
@dataclass
class UnifiedClaim:
    claim_id: str
    patient_id: Optional[str]
    procedure_code: str
    denial_reason: Optional[str]
    status: str
    submitted_at: str
    source_system: str
```

## 🎯 Business Impact

### **Revenue Recovery**

- Identifies claims with potential for successful resubmission
- Reduces revenue leakage from incorrectly denied claims
- Improves cash flow through faster claim resolution

### **Operational Efficiency**

- Automates manual claim review processes
- Reduces administrative overhead
- Provides data-driven insights for process improvement

### **Compliance & Risk Management**

- Ensures consistent application of business rules
- Maintains audit trail for regulatory compliance
- Reduces risk of manual errors

## 🔮 Future Enhancements

### **Planned Improvements**

1. **Machine Learning Integration**: Predictive denial analysis
2. **Real-time Processing**: Stream processing capabilities
3. **API Integration**: RESTful service endpoints
4. **Advanced Analytics**: Trend analysis and reporting
5. **Multi-tenant Support**: Healthcare system isolation

### **Scalability Roadmap**

- **Distributed Processing**: Apache Spark integration
- **Cloud Deployment**: AWS/Azure native deployment
- **Microservices Architecture**: Service decomposition
- **Real-time Monitoring**: Prometheus/Grafana integration

## 📞 Support

If you encounter any issues or have questions:

1. **Check the logs**: Review `claim_pipeline.log` for detailed error messages
2. **Verify data format**: Ensure your input files match the expected format
3. **Review configuration**: Check that business rules are set correctly
4. **Enable debug mode**: Use `logging.DEBUG` for detailed troubleshooting

---

**Happy Processing! 🚀**
