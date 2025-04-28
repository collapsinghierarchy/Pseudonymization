# Risk-Analysis
(Work in Progress)
## Introduction

The following reasoning is derived from current technical reports by ENISA, the European Data Protection Board (EDPB), the International Organization for Standardization (ISO), and our extensive experience in designing cryptographic protocols for both industrial and academic use cases.

### Relevant Resources

- [Pseudonymisation Techniques and Best Practices](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices)
- [Data Pseudonymisation: Advanced Techniques and Use Cases](https://www.enisa.europa.eu/publications/data-pseudonymisation-advanced-techniques-and-use-cases)
- [Technical Guidance on Pseudonymisation](https://op.europa.eu/en/publication-detail/-/publication/0e1ca64f-29c7-11e9-8d04-01aa75ed71a1/language-en)
- [Guidelines on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en)
- [Deploying Pseudonymisation Techniques](https://www.enisa.europa.eu/publications/deploying-pseudonymisation-techniques)
- [ISO 25237:2017 Health informatics — Pseudonymization](https://www.iso.org/obp/ui/#iso\:std\:iso:25237)
- [ISO/IEC 20889:2018 Privacy enhancing data de-identification terminology and classification of techniques](https://www.iso.org/obp/ui/#iso\:std\:iso-iec:20889)


### A Risk-Driven Approach

Paragraph 23 of the [Guidelines on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en) states:

> *Pseudonymisation is a technical and organisational measure that allows controllers and processors to reduce the risks to data subjects and meet their data protection obligations [...]*

A similar argument is presented on page 5 of [Pseudonymisation Techniques and Best Practices](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices):

> *Data controllers and processors should carefully consider the implementation of pseudonymisation following a risk-based approach [...]*

Likewise, on page 5 of the [Technical Guidance on Pseudonymisation](https://op.europa.eu/en/publication-detail/-/publication/0e1ca64f-29c7-11e9-8d04-01aa75ed71a1/language-en), the same reasoning is emphasized:

> *Clearly, pseudonymisation is not a prerequisite for all cases of personal data processing; hence, evaluating the relevant data protection risks (for each specific data processing case) is inherent to the decision on whether and how pseudonymisation can be implemented.*

Where there is no risk, pseudonymization is not required. Therefore, the first essential step is to conduct a risk analysis of the various data processing activities within your domain of responsibility before considering any pseudonymization measures. Ultimately, professionals with expertise in security and cryptography engineering will align with this risk-driven approach.

### Terminology

The following terminology is found in the sources of pseudonymization guidelines and best-practice reports.

- **Pseudonymization Domain**: A defined scope in which pseudonymization techniques are applied to protect data privacy.
- **Data Subject**: The individual whose personal data is being processed.
- **Controller**: The entity that determines the purposes and means of processing personal data.
- **Processor**: The entity that processes personal data on behalf of the controller.
- **Third Party**: Any entity other than the data subject, controller, or processor that is involved in the data processing chain.

We deviate in our risk analysis from this terminology mainly due to these terms having a significant legal definitional imprint, which makes them unsuitable for modeling risks based on technical details. However, this terminology is ultimately used for documentation purposes to translate the risk analysis and applied measures into the legal context, allowing auditors to attribute GDPR compliance.

### Technical Risk Analysis Terminology

We use the following terminology in our technical risk analysis:

- **Organisation Units**: These can be arbitrary organizational units within a company, such as an IT department or even the company as a whole. The structure depends largely on the authorization mechanisms in place, which define the boundaries of these units. We assume that these mechanisms function securely and can only be overridden by special actors (such as malware actors).
    - A set of actors belonging to the organization.

- **Actor**: Actors are individuals or entities that have relevance to the data by either having authorized access or the ability to escalate privileges to gain access. Special actors include malware variations that are not necessarily controlled by a human.
    - **(Read/Write) Access to Data Container (authorized/unauthorized)**: Defines whether the actor has the right to access or modify data.
    - **Impersonations**: Situations where an actor attempts to disguise themselves as another entity to gain unauthorized access.
    - **Organizational Affiliation(s)**: The formal relationship of an actor to the organization, which determines their level of trust and access.
    - **Background Knowledge**: Pre-existing knowledge that an actor may have, which can influence their ability to deduce or reconstruct data.

- **Target Data**: The data being processed and the focus of pseudonymization measures.
    - **Fields**: The smallest data units that can be individually accessed or processed.
    - **Origin**: The source from which the data originates.
    - **Destinations**: The endpoints or entities to which the data is transmitted or stored.

- **Data Container**: A technical unit responsible for the intermediate storage of data, which can pose a data breach risk. It is usually a data storage unit, where the data is stored and where it is frequently accessed from. Data is often cached and buffered throughout its transmission on various technical devices. However, in cases where transmission is conducted securely, we model these bufferings as transmission delays, treating them as if they were not buffered at all.
    - **Actors**

- **Transmission Channels**: The pathways through which data is transmitted between entities.
    - **Secure**: Encrypted or otherwise protected channels that ensure data confidentiality and integrity.
    - **Plain**: Unencrypted channels that leave data vulnerable to interception.

This structured terminology provides a clear framework for risk analysis while bridging technical aspects with legal compliance requirements.

### An Explorative-Iterative Approach  
The obvious question when conducting a risk analysis is, "Where do I start?" Our approach is highly flexible in this regard, as you will always arrive at the same overall picture, regardless of your initial consideration. In the future we will automatize this process such that no consultation of a security professional will be necessary. The following description may serve you as a starting point and give an imagination of how we are approaching the problem when consulting a client. They may sound simple from the security professionalism point of view, but our experience has shown that you have to adopt these processes to being as simple as possible and as complicated as necessary and this is the compromise we have developed.

### Start at the Source  
If you start at the source, you might ask yourself the following questions:  
- I need to request the following data from my customer.  
- Here are some APIs that request this data from my clients.  
- My client has this data stored locally, which we process later on.  

There are countless similar considerations, each providing valuable insights for the initial development of a data processing model. This model serves as the foundation for subsequent risk analysis.  

These considerations already define key aspects such as the data subject (actor) and their local data storage, as well as a transmission channel (secure/plain) and the actual data in question. From this point forward, we continue by asking further exploratory questions, tracing the data flow, and refining the overall architecture to identify potential breach points and points of failure.  

### Start in the Middle  
Other considerations that fall under the category of "starting in the middle" could include:  
- I have a database containing a large amount of personal data used in this application.  
- My employee processes the following personal data for a specific purpose.  
- This personal data needs to be transmitted to a third party.  

These considerations, much like starting at the source, already provide sufficient information to initiate the exploratory modeling process. By asking further questions, we can continue refining our understanding of the data flow and potential risks in the same iterative manner.  

### Start from an Attack Vector
What we also frequently hear during early consultations:

- I had an incident. The following data was stolen.  
- Which data could a malware access if one of my employees opens the wrong email?  
- This data is protected by a strong authorization mechanism.  

These considerations might be quite obvious to the reader, just as the previous considerations were. However, starting from an attack vector might become too exploratory. The previous examples were initially constrained to a specific data set or data subject, allowing us to quickly follow the data flow during the exploration phase.  

This time, we might encounter various datasets, and for each one, we must assess whether it contains personal data or is sensitive in another way, ensuring it remains out of reach of malware through technical safeguards. Ideally, the company already has a "data processing directory," which simplifies these kinds of assessments.

## Case Study: CRM
n the following, we present an example of the initial draft of such a risk modeling process. As you can see, we have abstracted a significant amount of explicit details that are potentially important for discussing technical risks. However, most of these details are implicitly understood by security professionals and do not require explicit modeling, such as the specific authorization mechanisms in place (e.g., single sign-on and its implementation). If the company has implemented these in a standardized manner, they do not need to be explicitly mentioned.
TODO: Here comes a diagram.
In the section about pseudonymization technqiues we present a pseudonymization achitecture that applies an encryption technique and a proper de-pseudonymization mechanism in order to close the identified data breach point of failure.
In the future we will release more of such examples that you can use by yourself for securing your data.
