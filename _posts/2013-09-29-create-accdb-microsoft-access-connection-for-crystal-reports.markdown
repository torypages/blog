---
layout: post
title: Create .accdb/.xlsx (Access/Excel) or SQL Server Connection for Crystal Reports
date: 2013-09-29 16:51:15.000000000 -04:00
categories:
- tech
tags:
- Access
- Crystal Reports
- Excel
- Microsoft
- Office
- windows
type: post
---
For Access/Excel, when creating a new connection for a Microsoft Access/Excel 2007 (+?) database you need to select "OLE DB (ADO)" as opposed to "Access/Excel (DAO)" which is for 2003 or perhaps earlier. After, you choose "Microsoft Office 12.0 Access Database Engine OLE DB Provider". I found this non-intuitive so I'm making note of it here. After selecting that, you will now have a menu where you can choose "Office Database Type" where you can select "Access" or "Excel". On this same menu, in the "Data Source" field you locate the actual data file. The rest is fairly intuitive.

For SQL Server, the steps are pretty similar, you select "OLE DB (ADO)" and then "Microsoft OLE DB Provider for SQL Server". You then need to fill out the connection information. After filling in the Server, User ID and the Password you will be able to select a database. The login credentials can be skipped if you choose Integrated Security which uses your Windows credentials.
