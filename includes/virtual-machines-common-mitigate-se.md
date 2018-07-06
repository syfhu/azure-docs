﻿---
 title: include file
 description: include file
 services: virtual-machines
 author: cynthn
 ms.service: virtual-machines
 ms.topic: include
 ms.date: 05/21/2018
 ms.author: cynthn;kareni
 ms.custom: include file
---


**Last document update**: 21 May 2018 3:00 PM PST.

The recent disclosure of a [new class of CPU vulnerabilities](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) known as speculative execution side-channel attacks has resulted in questions from customers seeking more clarity.  

Microsoft has deployed mitigations across all our cloud services. The infrastructure that runs Azure and isolates customer workloads from each other is protected.  This means that other customers running on Azure can't attack your application using these vulnerabilities.

In addition, Azure is expanding the use of [memory preserving maintenance](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#memory-preserving-maintenance) whenever possible, pausing the VM for up to 30 seconds while the host is updated or the VM is moved to an already updated host.  Memory preserving maintenance further minimizes customer impact and eliminates the need for reboots.  Azure will utilize these methods when making system-wide updates to the host.

> [!NOTE] 
On May 21, 2018, Google Project Zero and Microsoft announced a new subclass of speculative execution side-channel vulnerability known as Speculative Store Bypass. Additional defense in depth mitigations have been deployed across the Microsoft cloud infrastructure which directly address speculative execution vulnerabilities. More information is available here: https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180012 
>
> In late February 2018, Intel Corporation published updated [Microcode Revision Guidance](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/03/microcode-update-guidance.pdf) on the status of their microcode releases, which improve stability and mitigate against the recent vulnerabilities disclosed by [Google Project Zero](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). The mitigations put in place by Azure on [January 3, 2018](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) are not affected by Intel’s microcode update. Microsoft already put strong mitigations in place to protect Azure customers from other Azure virtual machines.  
>
> Intel’s microcode addresses variant 2 Spectre - [CVE-2017-5715](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5715) or branch target injection - to protect against attacks which would only be applicable where you run shared or untrusted workloads inside your VMs on Azure. Our engineers are testing the stability to minimize performance impacts of the microcode, prior to making it available to Azure customers.  As very few customers run untrusted workloads within their VMs, most customers will not need to enable this capability once released. 
>
> This page will be updated as more information is available.  






## Keeping your Operating Systems up-to-date

While an OS update is not required to isolate your applications running on Azure from other customers running on Azure, it is always a best practice to keep your OS versions up-to-date. The January 2018 and later [Security Rollups for Windows](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) contain mitigations for these vulnerabilities.

In the following offerings, here are our recommended actions to update your Operating System: 

<table>
<tr>
<th>Offering</th> <th>Recommended Action </th>
</tr>
<tr>
<td>Azure Cloud Services </td>	<td>Enable auto update or ensure you are running the newest Guest OS.</td>
</tr>
<tr>
<td>Azure Linux Virtual Machines</td> <td>Install updates from your operating system provider when available. </td>
</tr>
<tr>
<td>Azure Windows Virtual Machines </td> <td>Verify that you are running a supported antivirus application before you install OS updates. Contact your antivirus software vendor for compatibility information.<p> Install the [January security rollup](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Other Azure PaaS Services</td> <td>There is no action needed for customers using these services. Azure automatically keeps your OS versions up-to-date. </td>
</tr>
</table>

## Additional guidance if you are running untrusted code 

No additional customer action is needed unless you are running untrusted code. If you allow code that you do not trust (for example, you allow one of your customers to upload a binary or code-snippet that you then execute in the cloud within your application), then the following additional steps should be taken.  


### Windows 
If you are using Windows and hosting untrusted code, you should also enable a Windows feature called Kernel Virtual Address (KVA) Shadowing which provides additional protection against speculative execution side-channel vulnerabilities (specifically for variant 3 Meltdown, [CVE-2017-5754](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5754), or rogue data cache load). This feature is turned off by default and may impact performance if enabled. 
Follow [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) instructions for Enabling Protections on the Server. If you are running Azure Cloud Services, verify that you are running WA-GUEST-OS-5.15_201801-01 or WA-GUEST-OS-4.50_201801-01 (available starting on January 10, 2018) and enable the registry key via a startup task.


### Linux
If you are using Linux and hosting untrusted code, you should also update Linux to a more recent version that implements kernel page-table isolation (KPTI) which separates the page tables used by the kernel from those belonging to user space. These mitigations require a Linux OS update and can be obtained from your distribution provider when available. Your OS provider can tell you whether protections are enabled or disabled by default.



## Next steps

To learn more, see [Securing Azure customers from CPU vulnerability](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
