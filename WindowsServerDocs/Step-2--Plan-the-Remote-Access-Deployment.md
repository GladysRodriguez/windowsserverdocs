---
title: Step 2: Plan the Remote Access Deployment
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: 
  - techgroup-networking
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
---
# Step 2: Plan the Remote Access Deployment
After you plan for the infrastructure that you intend to use to set up your single Remote Access server for remote management of DirectAccess clients, you are ready to plan the settings that the Remote Access Setup Wizard will use.

> [!NOTE]
> Before you continue with these tasks, see [Step 1: Plan the Remote Access Infrastructure](assetId:///f7e327cd-5efb-4601-b567-c2576c414be1).

|Task|Description|
|--------|---------------|
|[Plan a client deployment strategy](#bkmk_2_1_client)|Decide which managed computers will be configured as DirectAccess clients.|
|[Plan a Remote Access server deployment strategy](#BKMK_PlanDeploy2)|Plan how to deploy the Remote Access server.|
|[Plan the infrastructure servers’ configurations](#bkmk_2_3_Inf_servers)|Plan the infrastructure servers in your Remote Access deployment, including the DirectAccess network location server, DNS servers, and DirectAccess management servers.|

## <a name="bkmk_2_1_client"></a>Plan a client deployment strategy
There are three decisions to make when you are planning your client deployment:

1.  Will DirectAccess be available to mobile computers only, or to every computer in a specified security group?

    When you configure DirectAccess clients in the DirectAccess Client Setup Wizard, you can choose to allow only mobile computers in specified security groups to connect to the server by using DirectAccess. If you restrict access to mobile computers, Remote Access automatically configures a WMI filter to ensure that the DirectAccess client GPO is applied only to mobile computers in the specified security groups. The Remote Access administrator requires permissions to create or modify group policy WMI filters to enable this setting.

2.  What security groups will contain the DirectAccess client computers?

    DirectAccess settings are contained in the DirectAccess client Group Policy Object \(GPO\). The GPO is applied to computers that are part of the security groups that you specify in the DirectAccess Client Setup Wizard. You can specify security groups that are contained in any supported domain. For more information, see [Plan Active Directory requirements](#BKMK_ActiveDirectory).

    Before you configure Remote Access, you need to create the security groups. You can add computers to the security group after you complete the Remote Access deployment. However, if you add client computers that reside in a different domain than the security group, the client GPO will not be applied to those clients. For example, if you created SG1 in domain A for DirectAccess clients, and you later add clients from domain B to this group, the client GPO will not be applied to clients in domain B.

    To avoid this issue, create a new client security group for each domain that contains client computers. Alternatively, if you do not want to create a new security group, run the **Add\-DAClient** Windows PowerShell cmdlet with the name of the new GPO for the new domain.

3.  What settings will you configure for the DirectAccess Network Connectivity Assistant?

    The DirectAccess Network Connectivity Assistant runs on client computers and provides additional information about the DirectAccess connection to end users. In the DirectAccess Client Setup Wizard, you can configure the following:

    -   **Connectivity verifiers**

        A default web probe is created that clients use to validate connectivity to the internal network. The default name is http:\/\/directaccess\-WebProbeHost.<domain\_name>. The name should be registered manually in DNS. You can create other connectivity verifiers that use other web addresses over HTTP or PING. For each connectivity verifier, a DNS entry must exist.

    -   **Help Desk email address**

        If end users experience DirectAccess connectivity issues, they can send an email that contains diagnostic information to the Remote Access administrator, who can troubleshoot the issue.

    -   **DirectAccess connection name**

        You can specify a DirectAccess connection name to help end users identify the DirectAccess connection on their computer.

    -   **Allow DirectAccess clients to use local name resolution**

        Clients require a means of resolving names locally. If you allow DirectAccess clients to use local name resolution, end users can use local DNS servers to resolve names. When end users choose to use local DNS servers for name resolution, DirectAccess does not send resolution requests for single label names to the internal corporate DNS server. It uses local name resolution instead \(by using the Link\-Local Multicast Name Resolution \(LLMNR\) and NetBios over TCP\/IP protocols\).

## <a name="bkmk_2_2_server"></a>Plan a Remote Access server deployment strategy
Decisions that you need to make when you are planning to deploy your Remote Access server include:

-   **Network topology**

    There are two topologies available when deploying a Remote Access server:

    -   **Two adapters**: With two network adapters, Remote Access can be configured with one network adapter connected directly to the Internet and the other connected to the internal network. Or alternatively, the server is installed behind an edge device, such as a firewall or a router. In this configuration one network adapter is connected to the perimeter network, and the other is connected to the internal network.

    -   **Single network adapter**: In this configuration the Remote Access server is installed behind an edge device, such as a firewall or a router. The network adapter is connected to the internal network.

    For more information, see [Plan network topology and server settings](assetId:///0064848b-b82e-4397-8fde-0c660c596076#bkmk_1_1_Network_svr_top_settings).

-   **Network adapters**

    The Remote Access Server Setup Wizard automatically detects the network adapters that are configured on the Remote Access server. You must make sure that the correct adapters are selected.

-   **IP\-HTTPS certificate**

    The Remote Access Server Setup Wizard automatically detects a certificate that is suitable for the IP\-HTTPS connection. The subject name of the certificate that you select must match the ConnectTo address. If you are using self\-signed certificates, you can choose to use a certificate that is created automatically by the Remote Access server.

-   **IPv6 prefixes**

    If the Remote Access Server Setup Wizard detects that IPv6 has been deployed on the network adapters, it automatically populates IPv6 prefixes for the internal network, an IPv6 prefix to assign to DirectAccess client computers, and an IPv6 prefix to assign to VPN client computers. If the automatically generated prefixes are not correct for your native IPv6 or ISATAP infrastructure, you must manually change them. For more information, see [Plan network topology and server settings](#BKMK_Network).

-   **Authentication**

    You can choose one of the following methods for authenticating DirectAccess clients to the Remote Access server:

    -   **User authentication**: You can enable users to authenticate with Active Directory credentials or with two\-factor authentication.

    -   **Computer authentication**: You can configure computer authentication to use certificates. Or the Remote Access server can act as a proxy for Kerberos authentication without requiring certificates.. For more information, see [Planning certification authorities and certificates](assetId:///0064848b-b82e-4397-8fde-0c660c596076#bkmk_1_2_CAs_and_certs).

    -   **Windows 7 clients** By default, client computers running [!INCLUDE[nextref_client_7](includes/nextref_client_7_md.md)] cannot connect to a Remote Access deployment running [!INCLUDE[win8_server_2](includes/win8_server_2_md.md)]. If you have clients running [!INCLUDE[nextref_client_7](includes/nextref_client_7_md.md)] in your organization that require remote access to internal resources, you can allow them to connect. Any client computers that you want to allow to access internal resources must be a member of a security group that you specify in the DirectAccess Client Setup Wizard.

        > [!NOTE]
        > Allowing clients running [!INCLUDE[nextref_client_7](includes/nextref_client_7_md.md)] to connect by using DirectAccess requires you to use computer certificate authentication.

-   **VPN configuration**

    Before you configure Remote Access, decide if you are going to provide VPN access to remote clients. You should provide VPN access if you have client computers in your organization that do not support DirectAccess connectivity \(for example, they are unmanaged or they run an operating system for which DirectAccess is not supported\). The Remote Access Server Setup Wizard allows you to configure how IP addresses are assigned \(by using DHCP or from a static address pool\) and how VPN clients are authenticated \(by using Active Directory or a RADIUS server\).

## <a name="bkmk_2_3_Inf_servers"></a>Plan the infrastructure servers’ configurations
Remote Access requires three types of infrastructure servers:

-   **Network location server**: For more information, see [Plan the network location server configuration](#BKMK_Location).

-   **DNS servers**: For more information, see [Plan DNS requirements](#BKMK_DNS).

-   **Management servers**: For more information, see [Plan management servers’ configurations](#BKMK_Management).

## <a name="BKMK_Links"></a>See also

-   [Step 1: Plan the Remote Access Infrastructure [client]](assetId:///f7e327cd-5efb-4601-b567-c2576c414be1)

