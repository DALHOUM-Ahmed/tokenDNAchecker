# DNA Similarity Check API - User Manual

## Introduction

Welcome to the DNA Similarity Check API! This service helps you assess the potential risk associated with token addresses by analyzing their smart contract bytecode. Our system learns from patterns and compares the DNA of a given address against its continuously updated DNA database to determine if it exhibits characteristics that might indicate malicious intent.


## Endpoints

The API provides the following endpoints:

### 1. Check DNA Similarity (`/check/:address`)

*   **Description:** This is the primary endpoint. It analyzes the DNA of the provided Ethereum address and determines if it exhibits characteristics that our system has learned to associate with potential risks.
*   **Method:** `GET`
*   **URL:** `/check/{address}`
    *   Replace `{address}` with the Ethereum address you want to check.
*   **Example:**
    ```
    GET /check/0x4674f73545f1db4036250ff8c33a39ad1678d864
    ```
*   **Responses:**
    *   **200 OK:**
        ```json
        {
          "address": "0x4674f73545f1db4036250ff8c33a39ad1678d864",
          "isMalicious": false
        }
        ```
        *   `isMalicious: false` - The address's DNA does not currently match patterns that our system associates with malicious activity.
        *   `isMalicious: true` - The address's DNA exhibits patterns that our system has learned to associate with potential malicious intent. Exercise caution.
    *   **500 Internal Server Error:**
        ```json
        {
          "error": "An error occurred while checking the address."
        }
        ```
        *   This indicates an unexpected error on our server. Please try again later or contact support if the issue persists.

### 2. Submit Address for Review (`/addMalicious/:address`)

*   **Description:** If you suspect an address might be associated with malicious activity, you can submit it to this endpoint. Our system will analyze its DNA and learn from the information.
*   **Method:** `GET`
*   **URL:** `/addMalicious/{address}`
    *   Replace `{address}` with the Ethereum address you want to submit.
*   **Example:**
    ```
    GET /addMalicious/0x7a226b9a2d01bdb2fee8d69c3181e1f6e25c6a66
    ```
*   **Responses:**
    *   **200 OK:**
        ```json
        {
          "message": "Address flagged as potentially malicious. Our system is learning and updating.",
          "address": "0x7a226b9a2d01bdb2fee8d69c3181e1f6e25c6a66"
        }
        ```
        *   The address has been received and our system will analyze it.
    *   **400 Bad Request:**
        *   **Possible Responses:**
            *   ```json
                {
                  "error": "Suspicious activity detected. Address exhibits patterns similar to known malicious addresses in our database."
                }
                ```
                *   The address you are trying to add is already considered malicious.
            *   ```json
                {
                  "error": "Could not fetch bytecode for the new address. Address might be invalid or not yet deployed."
                }
                ```
                *   The server was unable to retrieve the smart contract code for the given address. It might be an invalid address, or the contract might not be deployed yet.
            *   ```json
                {
                  "error": "Our system believes this address is legitimate based on learned patterns. Contact support for further assistance."
                }
                ```
                *   The address's DNA is very similar to addresses that our system believes are legitimate. If you have strong evidence to the contrary, please contact support.
            *   ```json
                {
                  "error": "Address already flagged as potentially malicious in our system."
                }
                ```
                *   The address was already submitted and flagged.
    *   **500 Internal Server Error:**
        ```json
        {
          "error": "An error occurred while updating our records."
        }
        ```
        *   An unexpected error occurred on the server.

### 3. Submit Address for Review (`/addLegit/:address`)

*   **Description:** You can submit addresses that you believe are legitimate to this endpoint. This helps our system learn and improve its analysis.
*   **Method:** `GET`
*   **URL:** `/addLegit/{address}`
    *   Replace `{address}` with the Ethereum address you want to submit.
*   **Example:**
    ```
    GET /addLegit/0x4674f73545f1db4036250ff8c33a39ad1678d864
    ```
*   **Responses:**
    *   **200 OK:**
        ```json
        {
          "message": "Address added to the list of potentially legitimate addresses. Our system is continuously learning.",
          "address": "0x4674f73545f1db4036250ff8c33a39ad1678d864"
        }
        ```
        *   The address has been received.
    *   **400 Bad Request:**
        *   **Possible Responses:**
            *   ```json
                {
                  "error": "Address already recognized as legitimate in our system."
                }
                ```
                *   The address is already considered legitimate by the system.
            *   ```json
                {
                  "error": "Could not fetch bytecode for the new address. Address might be invalid or not yet deployed."
                }
                ```
                *   The server could not retrieve the bytecode for the address.
            *   ```json
                {
                  "error": "Address already flagged as legitimate in our system."
                }
                ```
                *   The address was already submitted as legit.
    *   **500 Internal Server Error:**
        ```json
        {
          "error": "An error occurred while updating our records."
        }
        ```
        *   An unexpected server error occurred.

### 4. Remove Address from Review (`/remove/:address`)

*   **Description:** If you believe an address was incorrectly flagged by our system (either as potentially malicious or legitimate), you can request its removal using this endpoint.
*   **Method:** `GET`
*   **URL:** `/remove/{address}`
    *   Replace `{address}` with the Ethereum address you want to remove.
*   **Example:**
    ```
    GET /remove/0x7a226b9a2d01bdb2fee8d69c3181e1f6e25c6a66
    ```
*   **Responses:**
    *   **200 OK:**
        ```json
        {
          "message": "Address removal recorded. Our system is updating based on this change.",
          "address": "0x7a226b9a2d01bdb2fee8d69c3181e1f6e25c6a66"
        }
        ```
        *   The removal request has been recorded.
    *   **404 Not Found:**
        ```json
        {
          "error": "Address not found in our records."
        }
        ```
        *   The address was not found in the system's database.

## Important Considerations

*   **Learning System:** Our system is constantly learning and evolving. The results provided by this API may change over time as more data is analyzed.
*   **Contact Support:** If you encounter any issues or believe an address has been incorrectly flagged, please contact our support team. We appreciate your feedback!
