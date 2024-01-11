## Encryption

Encryption is the process of converting plaintext (readable data) into ciphertext (unreadable data) using an algorithm and a key. The purpose of encryption is to secure data, ensuring that only authorized parties with the appropriate decryption key can access and understand the original information.

In a web API context, encryption is often employed to protect sensitive information during data transmission. For example, encrypting data before sending it over HTTPS (TLS/SSL) ensures that the information is secure as it travels between the client and the server.

Here's an example of encryption in a Web API context using the ASP.NET Core framework. In this example, we'll focus on encrypting data transmitted over HTTPS.

```csharp
// Example Web API Controller
[ApiController]
[Route("api/secure-data")]
public class SecureDataController : ControllerBase
{
    [HttpGet]
    [Route("encrypted")]
    public IActionResult GetEncryptedData()
    {
        // Sensitive data to be encrypted
        string sensitiveData = "This is sensitive information.";

        // The data is automatically encrypted when using HTTPS (TLS/SSL)
        // No additional encryption coding is required in this case

        return Ok(sensitiveData);
    }
}
```

In this example, the sensitive data is automatically encrypted when transmitted over HTTPS. The encryption process is handled by the underlying TLS/SSL protocol, which encrypts the communication channel between the client and the server.

It's important to note that in a production environment, you should always use HTTPS to secure data transmitted over the web. The encryption provided by HTTPS ensures the confidentiality and integrity of the data during transmission. The encryption and decryption processes are transparent to the developer, as they are handled by the web server and the client's browser.

### SSL
An SSL (Secure Sockets Layer) certificate is a digital certificate that establishes a secure connection between a web server and a web browser. SSL certificates are used to encrypt the data transmitted between the server and the client, ensuring the confidentiality and integrity of the information.

SSL has been succeeded by TLS (Transport Layer Security), and the term "SSL certificate" is often used interchangeably with "TLS certificate." TLS is the updated and more secure version of the protocol.

There are various types of SSL/TLS certificates, and the choice of certificate depends on the specific needs and requirements of the website or application. The common types of SSL/TLS certificates include:

1. **Domain Validation (DV) Certificates:**
   - Provides basic encryption and verifies the ownership of the domain.
   - Suitable for personal websites and blogs.

2. **Organization Validation (OV) Certificates:**
   - Includes additional validation of the organization's identity, providing more trust.
   - Suitable for small to medium-sized businesses.

3. **Extended Validation (EV) Certificates:**
   - Requires rigorous validation of the organization's identity and provides the highest level of trust.
   - Triggers a green address bar in most browsers.
   - Suitable for e-commerce and financial websites.

4. **Wildcard Certificates:**
   - Secures a domain and all its subdomains with a single certificate.
   - Suitable for websites with multiple subdomains.

5. **Multi-Domain (SAN) Certificates:**
   - Allows securing multiple domain names with a single certificate.
   - Suitable for websites with several domains.

6. **Self-Signed Certificates:**
   - Created by the website owner without involving a Certificate Authority (CA).
   - Useful for testing and development environments but not recommended for production.

---
## Encoding
 

- Purpose: Encoding is used to represent data in a specific format for safe and efficient data transmission or storage.
- Security: Encoding is not designed for security; it's a reversible transformation that doesn't provide confidentiality. It's not meant to keep the data secret.
- Process: Encoding transforms data into a different format using a scheme that is publicly available. It is meant to ensure data integrity and proper handling but not to secure the data.

There are various types of encoding used in different contexts, each serving specific purposes. Here are some common types of encoding:

1. **URL Encoding (Percent-Encoding):**
   - **Purpose:** Used to encode special characters in URLs to ensure proper transmission.
   - **Example:** Space is encoded as `%20`, and other special characters have similar representations.

2. **Base64 Encoding:**
   - **Purpose:** Converts binary data into ASCII text, commonly used for encoding binary files in text-based formats.
   - **Example:** `Hello, World!` can be encoded as `SGVsbG8sIFdvcmxkIQ==`.

4. **JSON Encoding:**
   - **Purpose:** Represents data in JSON format for communication between systems.
   - **Example:** `{ "name": "John", "age": 25 }`.

5. **UTF-8 Encoding:**
   - **Purpose:** Represents characters in Unicode format, widely used for character encoding.
   - **Example:** Unicode character U+00E9 (é) is represented as `C3 A9` in UTF-8.
 

 ---

 ## MediaTypeFormatter 
 - is a class responsible for handling the serialization and deserialization of data between the HTTP request/response and .NET objects.