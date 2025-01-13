# Exchange Packet Streamer

A simple .NET application that connects to an exchange server over TCP, retrieves trading packets, identifies missing sequence numbers, requests the missing packets, and stores the complete data in a JSON file.

## Features

- Connects to a TCP server to stream exchange packets.
- Identifies and reports missing sequence numbers.
- Requests missing packets from the server.
- Writes all packets (including missing ones) to a JSON file.
- Packets are parsed and stored in an object for easy access.
- JSON file is ordered by packet sequence.

## Getting Started

### Prerequisites

Ensure you have the following installed on your system:

- [.NET 6.0 SDK or higher](https://dotnet.microsoft.com/download)
- A running TCP exchange server that sends the expected packets. This application connects to `127.0.0.1:3000`.

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/exchange-packet-streamer.git


2. Navigate into the project directory:
   
   ```bash
   cd exchange-packet-streamer

3. Restore the project dependencies:

   ```bash
   dotnet restore
   
4. Run the application:

   ```bash
   dotnet run

## Input

The application connects to the exchange server running at `127.0.0.1:3000` on port 3000. The server should be sending packets of the following structure:

- Symbol (4 bytes)
- Buy/Sell indicator (1 byte)
- Quantity (4 bytes)
- Price (4 bytes)
- Sequence number (4 bytes)

## Output

The application will output the following:

- The completed list of packets will be written to a file called `all_packets.json`.

You can inspect the packets in the generated JSON file, which will contain the following fields for each packet:

- `Symbol`: The trading symbol (string).
- `BuySell`: Indicates whether it's a buy ('B') or sell ('S') operation (char).
- `Quantity`: The quantity of the trade (integer).
- `Price`: The price at which the trade occurred (integer).
- `Sequence`: The sequence number of the packet (integer).

### Example of the JSON file structure:

    ```json
    [
      {
        "Symbol": "AAPL",
        "BuySell": "B",
        "Quantity": 100,
        "Price": 150,
        "Sequence": 1
      },
      {
        "Symbol": "GOOG",
        "BuySell": "S",
        "Quantity": 50,
        "Price": 2800,
        "Sequence": 2
      }
    ]


## Code Explanation

### Main Method:
- Retrieves all packets from the server.
- Checks for missing sequences.
- Requests missing packets.
- Writes the complete packet data to a JSON file.

### GetAllPackets:
- Connects to the server and receives packets.
- The packets are parsed and stored in a list.

### FindMissingSequences:
- Finds the missing sequence numbers between the minimum and maximum sequence found in the packets.

### RequestMissingPackets:
- For each missing sequence, it sends a request to the server to fetch the missing packets.

### WritePacketsToJson:
- Writes the received packets to a JSON file in the current directory.

### ExchangePacket Class:
- Defines the structure of the trading packet with fields like `Symbol`, `Buy/Sell`, `Quantity`, `Price`, and `Sequence`.


