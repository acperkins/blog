# Exchange Used Disk Space warning
2020-10-23 — Anthony Perkins

---

When sending an email from a CentOS server via an Exchange relay, I got the following warning from
the `mailq` command and in `/var/log/maillog`:

```
452 4.3.1 Insufficient system resources (UsedDiskSpace[C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data\Queue]) (in reply to end of DATA command))
```

I checked the Event Viewer on the mail server in question, and **Event 15006, MSExchangeTransport**
had been logged:

```
Microsoft Exchange Transport is rejecting message submissions because the available disk space has dropped below the configured threshold.The following resources are under pressure:
Used disk space ("C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data\Queue")
Used disk space ("C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data")
Overall Resources

The following components are disabled due to back pressure:
Mail resubmission from the Message Resubmission component.
Messaging Database
Mail submission from Pickup directory
Mail submission from Replay directory
Mail resubmission from the Shadow Redundancy Component
Message Replication Component
Inbound mail submission from the Internet

The following resources are in normal state:
Private bytes
System memory
Version buckets[C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data\Queue\mail.que]
Jet Sessions[C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data\Queue\mail.que]
Checkpoint Depth[C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data\Queue\mail.que]
Queue database and disk space ("C:\Program Files\Microsoft\Exchange Server\V15\TransportRoles\data\Queue\mail.que")
```

It turns out that Exchange has thresholds for disk usage, known as _back pressure_. If the disk is
over approximately 90% capacity (which this disk was) then mail is not accepted from relay hosts. At
99% capacity, mail flow stops completely.

We had been seeing intermittent mail flow as the disk space fluctuated around the threshold, going
over for a short while and then dropping back under, but eventually the disk filled up enough that
mail relaying stopped completely.

Once I increased the disk capacity on the drive in question, mail started flowing again.

---

Copyright © 2020 Anthony Perkins. Some rights reserved.
