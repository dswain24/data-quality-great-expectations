```markdown
# Data Quality Validation with Great Expectations (Dummy CSV)
## Overview
This project demonstrates basic data quality validation using Great Expectations on a dummy CSV file
containing
customer data. Expectations are defined to assert non-null fields and valid ranges.
## Tech Stack
- Python
- Great Expectations
- Dummy CSV data
## How It Works
1. `great_expectations init` sets up the configuration folder.
2. An expectation suite (e.g., `customers_suite`) is defined for `sample_data.csv`.
3. `run_validation.py` runs a checkpoint that validates `sample_data.csv` against the expectations.
## Setup and Run Steps
1. Create and activate a virtual environment (if not already active).
2. Install Great Expectations:
```bash
pip install great_expectations
```
3. Run the init command:
```bash
great_expectations init
```
4. Place `sample_data.csv` into the appropriate `data` folder used by Great Expectations (or configure a
datasource
that points to your `data-quality-great-expectations/data` directory).
5. Create a new expectation suite:
```bash
great_expectations suite new
```
- Choose interactive mode and name the suite `customers_suite`.
- Follow prompts to define expectations, such as non-null `customer_id`, valid `age` ranges, etc.
6. Run the validation script:
```bash
python run_validation.py
```
## Common Errors and Debugging Tips
**Error 1: Datasource or data asset not found**
- Cause: The datasource name or file path in the checkpoint configuration does not match your actual
setup.
- Fix:
- Open `great_expectations.yml` and check datasource names.
- Ensure `data_asset_name` in the checkpoint matches your CSV file name exactly.
**Error 2: Expectation suite not found**
- Cause: Typo in `expectation_suite_name` or the suite was not created.
- Fix:
- Run `great_expectations suite list` to see existing suites.
- Make sure the name in `run_validation.py` matches exactly.

```
run_validation.py
```python
from great_expectations.checkpoint import SimpleCheckpoint
from great_expectations.data_context import DataContext
if __name__ == '__main__':
context = DataContext()
checkpoint_config = {
'name': 'customers_checkpoint',
'config_version': 1.0,
'class_name': 'SimpleCheckpoint',
'run_name_template': '%Y-%m-%d-%H-%M-%S-customers',
'validations': [
{
'batch_request': {
'datasource_name': 'data__dir', # adjust to match your datasource name
'data_connector_name': 'default_inferred_data_connector_name',
'data_asset_name': 'sample_data.csv',
'limit': 1000,
},
'expectation_suite_name': 'customers_suite',
}
],
}
checkpoint = SimpleCheckpoint(
name=checkpoint_config['name'],
data_context=context,
**checkpoint_config
)
result = checkpoint.run()
print(result)
```
