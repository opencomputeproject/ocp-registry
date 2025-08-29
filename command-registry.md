---
title: "OCP Command Registry"
version: 1.0
type: REGISTRY
project: Security
authors: [(See Acknowledgements section)]
---

# OCP Command Registry

This document contains an informative list of Command Codes defined by OCP specifications. This document serves as only a reference with short descriptions. The source of the normative references of the command codes and their definitions are in the OCP specifications themselves.

## Acknowledgements

The Contributors of this Registry would like to acknowledge the following:

- Fabrizio D'Amato (AMD)
- Jeff Andersen (Google)
- Steven Bellock (NVIDIA)

## Command Code Registry

| Command Code | Command Name             | Defining Specification                             | Description                                                        |
| :----------- | :----------------------- | :------------------------------------------------- | :----------------------------------------------------------------  |
| 0x00         | Reserved                 | -                                                  | Reserved for future use                                            |
| 0x01         | GET_ENVELOPE_SIGNED_CSR  | OCP Device Identity Provisioning                   | Requests a replay-protected CSR envelope-signed by attestation key |
| 0x02         | GET_EAT                  | OCP Profile for IETF Entity Attestation Token      | Requests an Entity Attestation Token conforming to OCP Profile     |
| 0x03-0xFF    | Reserved                 | -                                                  | Reserved for future assignment                                     |

## Command Assignment Process

New OCP specifications that require command codes **MUST**:

1. Request command code assignment from the OCP Security Working Group
2. Provide command name and brief description
3. Reference the defining specification
4. Update this registry upon approval

## Transport Binding Requirements

Each transport protocol binding **MUST** define:

1. How to frame OCP command requests and responses
2. How to convey OCP success responses with full payload
3. How to convey OCP error codes using the transport's native error mechanisms

### SPDM Transport Binding

#### Command Framing
- **Request**: Use SPDM VENDOR_DEFINED_REQUEST format
- **Response**: Use either SPDM VENDOR_DEFINED_RESPONSE (success) or SPDM ERROR (failure)

#### Standard Fields
- StandardID field **MUST** contain 4 (IANA)
- VendorID field **MUST** contain 42623 (OCP)

#### Success Response Structure
- Use SPDM VENDOR_DEFINED_RESPONSE format
- First byte of VendorDefinedReqPayload/VendorDefinedRespPayload **MUST** contain the CommandVersion
- Second byte of VendorDefinedReqPayload/VendorDefinedRespPayload **MUST** contain the CommandCode
- OCP command request/response structures form the remainder of the payload

#### Error Response Structure
- Use SPDM ERROR message format
- Include OCP error code in SPDM ExtendedErrorData when available
- Follow SPDM error response procedures
- See OCP Error Registry for available error codes

### Future Transport Bindings

Other transport protocols **MAY** define their own bindings provided they:

- Maintain semantic equivalence of request and response structures
- Preserve all required fields and their meanings
- Implement appropriate error reporting using transport-native mechanisms
- Document any transport-specific adaptations

## Cross-Specification References

### GET_ENVELOPE_SIGNED_CSR (0x01)
- **Defined in:** OCP Device Identity Provisioning Specification
- **Purpose:** Requests a freshness-protected (replay-resistant) Certificate Signing Request that is envelope-signed by an attestation key to establish trust in a device's identity keypair
- **Reference:** See Device Identity Provisioning specification for full command definition

### GET_EAT (0x02)
- **Defined in:** OCP Profile for IETF Entity Attestation Token Specification
- **Purpose:** Retrieves attestation evidence in OCP EAT format
- **Reference:** See OCP EAT Profile specification for full command definition