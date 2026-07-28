# Description

An information disclosure vulnerability exists in the certificate validation workflow that allows an unauthenticated attacker to obtain the original version of a course completion certificate containing personally identifiable information (PII).

The attack can be initiated using information commonly exposed in publicly shared certificates, such as those posted on professional networking platforms. Although sensitive fields including CPF and RG may be visually redacted in the shared image, the certificate still exposes the certificate holder's full name, date of birth, and a QR Code referencing the official certificate verification service.

By scanning the QR Code, an attacker is redirected to the official certificate validation portal, which provides a search mechanism based solely on the certificate holder's full name and date of birth. Since these attributes are publicly visible in the shared certificate, they can be supplied to the validation interface without requiring authentication or authorization. The application subsequently returns the original certificate in downloadable form, exposing information intentionally concealed in the publicly shared version, including the complete CPF, complete RG, and the certificate holder's signature.

The vulnerability results from insufficient access control and excessive disclosure of personal information during the certificate validation process, allowing unauthorized retrieval of documents containing sensitive personal data.

# Affected endpoint
https://www.fiap.com.br/graduacao/consulta-publica-de-diplomas/?chave=@(Html.Raw(((java.lang.String)field_DadosRegistroSegurancaCodigoValidacao.getValue())))/

# Payload Example
Full Name and Birth Date

# PoC

<img width="970" height="744" alt="CVE FIAP" src="https://github.com/user-attachments/assets/66e64170-0551-453f-bcc9-58d335ffa1fb" />
<img width="967" height="642" alt="2" src="https://github.com/user-attachments/assets/18c03011-c735-48d1-98ff-0dbffde3aa95" />
<img width="1278" height="747" alt="3" src="https://github.com/user-attachments/assets/a645b26c-dbfa-4353-bb21-fb1e448b995c" />
<img width="1280" height="750" alt="4" src="https://github.com/user-attachments/assets/c4d87f5e-6005-4eb6-bc29-540d54192008" />
<img width="1277" height="753" alt="5" src="https://github.com/user-attachments/assets/e1a0aa54-44a3-41e4-a8c7-8097da837c59" />
<img width="1278" height="751" alt="6" src="https://github.com/user-attachments/assets/1913cc3a-6000-4901-b4d7-8822fe64645c" />
<img width="1278" height="752" alt="7" src="https://github.com/user-attachments/assets/060d874a-4fcd-42bf-aed2-3511e6de4a1a" />
<img width="1278" height="750" alt="8 1" src="https://github.com/user-attachments/assets/be207ab3-3e9e-4002-a7ff-03fe65100a06" />

# Impact

Successful exploitation enables unauthorized disclosure of personally identifiable information without requiring valid credentials or elevated privileges. An attacker can obtain complete identification data, including CPF, RG, date of birth, full legal name, and the certificate holder's signature, significantly increasing the risk of identity theft, social engineering, impersonation, document forgery, and targeted phishing attacks.

Because course completion certificates are frequently published on professional networking platforms and other public websites, an attacker can leverage Open Source Intelligence (OSINT) techniques and search engine indexing (e.g., Google Dorking) to identify publicly accessible certificates at scale. The exposed full name, date of birth, and QR Code present in these certificates can then be used to systematically retrieve the corresponding unredacted certificates from the validation portal. This enables automated collection of sensitive personal information from multiple individuals, facilitating the creation of a database containing PII obtained from otherwise redacted certificates. Such datasets may subsequently be used to support identity theft, credential profiling, targeted phishing campaigns, and other malicious activities.

# Recommendation

The certificate validation mechanism should be redesigned to enforce proper authorization controls and minimize exposure of sensitive information. Retrieval of the complete certificate should not be possible using predictable personal attributes such as full name and date of birth. Instead, certificate validation should rely on high-entropy, non-predictable identifiers or cryptographically secure validation tokens that cannot be derived from publicly available information. The validation service should expose only the minimum amount of information required to confirm certificate authenticity, avoiding disclosure of personally identifiable information such as CPF, RG, signatures, or other sensitive fields. Access to the original certificate should require appropriate authentication or proof of ownership, and downloadable documents intended for public verification should permanently mask sensitive personal information. Additionally, the application should implement monitoring and rate limiting to detect and prevent automated enumeration attempts.

# CVSS 3.1
Score Medium 6.8 -> AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:L/A:N
