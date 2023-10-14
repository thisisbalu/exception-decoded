---
title: "Understanding and Resolving SSLPeerUnverifiedException in Java"
date: 2023-10-13 22:34:28 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, javax.net.ssl, java-se]
mermaid: true
toc: true
---


Exception handling is a critical aspect of application development. In the world of Java, SSLPeerUnverifiedException is one of the most encountered exceptions, particularly when dealing with secure SSL connections. This article takes a deep dive into what the SSLPeerUnverifiedException is, why it occurs, and how to resolve it.

## Understanding SSLPeerUnverifiedException 

Before diving into SSLPeerUnverifiedException, let's quickly grasp the context in which this exception arises. Simply put, `javax.net.ssl.SSLPeerUnverifiedException` gets thrown when the SSL peer couldn't get verified. 

It's noteworthy that this PeerUnverifiedException is an extension of the `javax.net.ssl.SSLException`, and its bit of a troublemaker while handling secure SSL connection as it interferes with the handshake between the client and server.

```java
public class SSLPeerUnverifiedException extends SSLException {

    private static final long serialVersionUID = -8919512675000600547L;

    /**
     * Constructs an exception reporting that the SSL peer's
     * identity has not been verified.
     *
     * @param reason describes the problem.
     */
    public SSLPeerUnverifiedException(String reason) {
    super(reason);
    }
}
```

The principal cause raising the SSLPeerUnverifiedException is the failure of SSL certificate verification by the Java application. It's mainly due to the absence of the corresponding certificate in the truststore of Java.

## Why Does SSLPeerUnverifiedException Occur?

The SSL/TLS protocol uses certificates to authenticate the server in the communication. The protocol mandates that each certificate must be signed by a trusted Certificate Authority (CA). In the case of Java, the trusted certificates are stored in a keystore file called `cacerts`. When a certificate can't be trusted (missing in cacerts), Java throws an SSLPeerUnverifiedException.

Let's understand this via an example.

```java
try {
    URL url = new URL("https://untrusted-cert-server.com");
    URLConnection urlConnection = url.openConnection();
    BufferedReader in = new BufferedReader(
          new InputStreamReader(urlConnection.getInputStream()));
    in.close();
} catch (SSLPeerUnverifiedException e) {
     System.out.println("SSLPeerUnverifiedException Caught: " + e.getMessage());
} catch (Exception e) {
    System.out.println("General Exception Caught: " + e.getMessage());
}
```

The URL (`https://untrusted-cert-server.com`) in the above example references a server with a non-trusted SSL certificate. When Java doesn’t find the referenced certificate in its truststore, it throws SSLPeerUnverifiedException.

## Resolving SSLPeerUnverifiedException

### 1. Updating the Truststore

The most prominent solution to resolve this exception is to update the Java Truststore (`cacerts`) with the untrusted certificate. We might need to download the certificate from the server manually and add it into the Truststore.

Following is a step-by-step guide of how to do it:

1. Download the certificate by connecting to the server with OpenSSL:

    ```bash
    echo | openssl s_client -servername hostname -connect host:port 2>/dev/null | openssl x509 > certificate.crt
    ```

2. Import the certificate to the Java Truststore:

    ```bash
    keytool -importcert -file certificate.crt -keystore ${JAVA_HOME}/lib/security/cacerts
    ```

### 2. Trust All Certificates 

In some scenarios, for development or testing purposes, we might want to trust all certificates, irrespective of whether they are in the Truststore. 

However, **this is highly discouraged for production environments as it makes the application vulnerable to Man In The Middle (MITM) attacks**.

But in case this is what you want, you create a TrustManager that does not validate certificate chains like the default TrustManager does. Then, you initialize a `SSLContext` with this TrustManager:

```java
TrustManager[] trustAllCerts = new TrustManager[]{
    new X509TrustManager() {
        public java.security.cert.X509Certificate[] getAcceptedIssuers() {
            return null;
        }

        public void checkClientTrusted(
            java.security.cert.X509Certificate[] certs, String authType) {
        }

        public void checkServerTrusted(
            java.security.cert.X509Certificate[] certs, String authType) {
        }
    }
};
try {
    SSLContext sc = SSLContext.getInstance("SSL");
    sc.init(null, trustAllCerts, new java.security.SecureRandom());
    HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
} catch (GeneralSecurityException e) {
    // Do something...
}
```

Regardless of the approach you take, always remember that dealing with an SSLPeerUnverifiedException is a necessary measure to maintain robust SSL/TLS security in your application. 

## Conclusion

In this article, we navigated through the SSLPeerUnverifiedException in Java, why it happens, and how we can resolve it. However, SSL/TLS is a vast territory, and much is left to explore. Keep experimenting and keep learning!

## Reference
1. [SSLPeerUnverifiedException Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/javax/net/ssl/SSLPeerUnverifiedException.html)
2. [How SSL Works](https://www.ibm.com/docs/en/zos/2.1.0?topic=SSB27U_2.1.0/global_sec/sslconcepts.html)
3. [SSLContext Java Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/javax/net/ssl/SSLContext.html)