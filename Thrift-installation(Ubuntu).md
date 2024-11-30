# Thrift Calculator Example

This project demonstrates a simple calculator service using Apache Thrift with Python. It includes a server that provides addition and subtraction operations, and a client that connects to the server to perform these operations.

## Prerequisites

- Python (2.7+ or 3.x)
- pip (Python package installer)
- Apache Thrift

## Installation

1. Install Apache Thrift:

   For Ubuntu/Debian:
   ```
   sudo apt-get update
   sudo apt-get install thrift-compiler
   ```


2. Install the Thrift Python library:

   ```
   pip install thrift
   ```

   If you're using Python 3, you might need to use `pip3` instead:

   ```
   pip3 install thrift
   ```

   If you encounter permission issues, you can use the \`--user\` flag:

   ```
   pip install thrift --user
   ```

## Project Structure

- `calculator.thrift`: Thrift definition file for the calculator service
- `server.py`: Python script for the calculator server
- `client.py`: Python script for the calculator client
- `gen-py/`: Directory containing Thrift-generated code (created after running Thrift compiler)

## Running the Example

1. Generate the Thrift code:

   ```
   thrift -r --gen py calculator.thrift
   ```

   This will create a \`gen-py\` directory with the generated code.

2. Run the server:

   ```
   python server.py
   ```

   You should see the message "Starting the server..."

3. In another terminal, run the client:

   ```
   python client.py
   ```

   You should see the results of the addition and subtraction operations.

## Troubleshooting

If you encounter a "No module named thrift.Thrift" error, try the following:

1. Find out where Thrift is installed:

   ```
   pip show thrift
   ```

2. Add the Thrift installation location to your PYTHONPATH:

   ```
   export PYTHONPATH=$PYTHONPATH:/home/tishbian/.local/lib/python2.7/site-packages/thrift
   ```



