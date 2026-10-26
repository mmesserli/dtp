# Flow of integrated signing solution

The signing can be integrated easily into external applications. The signing process can be scheduled directly within the existing user flow of the external application by calling a REST API. Once a document basket is created, the end user can start signing. The external application will be notified by webhook on the current status of the document basket. Once the signing is completed the external application can download all signed documents per REST API.

```mermaid
sequenceDiagram
    actor Enduser as end user
    participant ExternalApplication as external application
    participant m_IDeal as signing service

    Enduser->>ExternalApplication: Initialize signing process
    ExternalApplication->>m_IDeal: create a document basket <br> POST /{tenantId}/document-baskets
    m_IDeal->>ExternalApplication: return document basket id
    ExternalApplication->>m_IDeal: upload documents into the document basket <br> POST /{tenantId}/document-baskets/{basketId}/documents

    alt Enable signer last document feature
        ExternalApplication->>m_IDeal: Call to notify last document <br> POST /{tenantId}/document-baskets/{documentBasketId}/last-document
    end

    alt Smart-link is not used
        ExternalApplication->>m_IDeal: Request new signing link (can be used once, 4 min validity) before user starts signing <br> POST /{tenantId}/document-baskets/{documentBasketId}/sign-links
        m_IDeal->>ExternalApplication: Respond with new signing link (can be used once, 4 min validity)
    end

    ExternalApplication->>Enduser: return the url to start signing
    Enduser->>m_IDeal: start the signing process
    m_IDeal->>ExternalApplication: the signing status will be sent over signing notification
    ExternalApplication->>m_IDeal: get signed documents <br> GET /{tenantId}/document-basket/{documentBasketId}/files.zip
    m_IDeal->>ExternalApplication: return signed documents


```
