Amazon RDS supports using Transparent Data Encryption (TDE) to encrypt stored data on your DB instances running Microsoft SQL Server. TDE automaticly encrypts data before it is written to storage and automaticly decrypts data when the data is read from storage.
Amazon RDS supports TDE for the following SQL Server versions and editions:
- SQL Server 2017 Enterprise Edition
- SQL Server 2016 Enterprise Edition
- SQL Server 2014 Enterprise Edition
- SQL Server 2012 Enterprise Edition
- SQL Server 2008 R2 Enterprise Edition

To enable transparent data encryption for a DB instance running SQL Server, specify the TDE option in an Amazon RDS option group associated with that DB instance.

[More info](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.TDE.html)