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

- `calculator.thrift` Paste the following Code:
   ```
   namespace py calculator
   service Calculator {
    i32 add(1: i32 num1, 2: i32 num2),
    i32 subtract(1: i32 num1, 2: i32 num2)
   }
   ```
- `server.py` Paste the following Code:
  ```
  from __future__ import print_function

   import sys
   sys.path.append('gen-py')

   from calculator import Calculator
   from calculator.ttypes import *

   from thrift.transport import TSocket
   from thrift.transport import TTransport
   from thrift.protocol import TBinaryProtocol
   from thrift.server import TServer

   class CalculatorHandler:
    def add(self, num1, num2):
        print("Adding {0} and {1}".format(num1, num2))
        return num1 + num2

    def subtract(self, num1, num2):
        print("Subtracting {1} from {0}".format(num1, num2))
        return num1 - num2

   if __name__ == '__main__':
    handler = CalculatorHandler()
    processor = Calculator.Processor(handler)
    transport = TSocket.TServerSocket(port=9090)
    tfactory = TTransport.TBufferedTransportFactory()
    pfactory = TBinaryProtocol.TBinaryProtocolFactory()

    server = TServer.TSimpleServer(processor, transport, tfactory, pfactory)

    print("Starting the server...")
    server.serve()
    print("Done.")


   ```
- `client.py` Paste the following Code:
   ```
   from __future__ import print_function

   import sys
   sys.path.append('gen-py')

   from calculator import Calculator
   from calculator.ttypes import *

   from thrift import Thrift
   from thrift.transport import TSocket
   from thrift.transport import TTransport
   from thrift.protocol import TBinaryProtocol

   try:
    transport = TSocket.TSocket('localhost', 9090)
    transport = TTransport.TBufferedTransport(transport)
    protocol = TBinaryProtocol.TBinaryProtocol(transport)
    client = Calculator.Client(protocol)

    transport.open()

    sum_result = client.add(5, 3)
    print("5+3={0}".format(sum_result))

    diff_result = client.subtract(10, 4)
    print("10-4={0}".format(diff_result))

    transport.close()

   except Thrift.TException as tx:
    print("Thrift Exception: {0}".format(tx.message))


   ```

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



