---
title: "OCP Error Registry"
version: 1.0
type: REGISTRY
project: Security
authors: [(See Acknowledgements section)]
---

# OCP Error Registry

This document contains an informative list of Error Codes defined by OCP specifications.

## Acknowledgements

The Contributors of this Registry would like to acknowledge the following:

- Fabrizio D'Amato (AMD)
- Jeff Andersen (Google)
- Steven Bellock (NVIDIA)

## Error Code Registry

| Error Code | Error Name                    | Description                                           |
| :--------- | :---------------------------- | :---------------------------------------------------- |
| 0x00       | SUCCESS                       | Command completed successfully                        |
| 0x01       | GENERAL_ERROR                 | Unspecified error occurred                            |
| 0x02       | INVALID_PARAMETER             | One or more command parameters are invalid            |
| 0x03       | INVALID_LENGTH                | Specified length field is outside valid range         | 
| 0x04       | INVALID_IDENTIFIER            | Specified identifier not found or not available       |
| 0x05       | OPERATION_FAILED              | Requested operation could not be completed            |
| 0x06       | INSUFFICIENT_RESOURCES        | Insufficient memory or processing resources           |
| 0x07       | UNSUPPORTED_OPERATION         | Requested operation not supported by implementation   |
| 0x08       | DEVICE_NOT_READY              | Device not in appropriate state for command           |
| 0x09       | INVALID_COMMAND_VERSION       | Command version not supported                         |
| 0x0A       | INVALID_PAYLOAD_SIZE          | Command payload size exceeds limits                   |
| 0x0B       | TIMEOUT                       | Operation timed out                                   |
| 0x0C       | ACCESS_DENIED                 | Insufficient privileges for requested operation       |
| 0x0D       | RESOURCE_UNAVAILABLE          | Required resource is not accessible                   |
| 0x0E       | POLICY_VIOLATION              | Command violates device or system policy              |
| 0x0F       | INVALID_STATE                 | Device or resource in invalid state for operation     |
| 0x10-0x7F  | Reserved (Generic)            | Reserved for future generic error assignments         |
| 0x80-0xBF  | Reserved (Security)           | Reserved for security-specific error codes            |
| 0xC0-0xFF  | Reserved (Project-Specific)   | Reserved for project-specific error codes             |

## Error Code Categories

### Generic Errors (0x00-0x7F)
These error codes can be used across all OCP projects and are not specific to any particular domain. They cover common failure conditions that apply to any command-based protocol.

### Security-Specific Errors (0x80-0xBF)
Reserved for security-specific error conditions such as:
- Cryptographic operation failures
- Certificate validation errors
- Authentication/authorization failures
- Security policy violations

### Project-Specific Errors (0xC0-0xFF)
Reserved for errors specific to particular OCP projects (e.g., networking, storage, compute).

## Error Assignment Process

New OCP specifications that require error codes **MUST**:

1. Request error code assignment from the appropriate OCP working group
2. Provide error name and detailed description
3. Reference the defining specification
4. Specify the intended category (Generic, Security, Project-Specific)
5. Update this registry upon approval

## Error Reporting Requirements

### Transport-Agnostic Error Handling

OCP error codes are defined independently of transport protocols. Each transport binding **MUST** specify how to convey OCP error codes using the transport's native error mechanisms.

### SPDM Error Reporting

When using SPDM transport, OCP errors are reported as follows:

#### Protocol-Level Errors
- Use standard SPDM ERROR message format
- Set appropriate SPDM ErrorCode (e.g., VendorDefined, InvalidRequest)
- Follow SPDM error response procedures

### Other Transport Error Reporting

Future transport bindings **MAY** use different approaches such as:
- **Native Error Fields**: Include OCP error code in response structure
- **Status Responses**: Return either full response or error-only response
- **Exception Mechanisms**: Use transport-native exception handling

## Error Handling Best Practices

### For OCP Command Implementations
- **MUST** use appropriate OCP error codes for all failure conditions
- **SHOULD** use generic error codes when possible for broader compatibility
- **SHOULD** provide meaningful error details when possible