# Flask App with Pytest

## Getting started

To run the project follow these commands:

```
cd 1.1-ci
pip install -r requirements.txt
python3 -m pytest --setup-plan --disable-warnings
python3 -m pytest --disable-warnings --collect-only
python3 -m coverage run -m pytest --disable-warnings -v
```
