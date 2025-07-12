
The current OSM API (v0.5) is not designed to handle history, changesets and rollback functions. Implementing this is an important step towards a larger user base.

Because an API change of this magnitude has great repercussions on a large part of the OSM software stack, it needs to be well prepared and discussed. This is why we decided to allocate a modest part of the NLNet funding to a "hack-a-thon".

The hack-a-thon brought together ten of the key developers in the OSM community from all over Europe. It was held in a London office provided by Steve Coast on May 3rd and 4th, 2008. The NLNet funding was allocated to travel and accomodation expenses for the participants, who came over from Germany, Austria, the Netherlands and the UK.

What follows below is a write-up of the results of the hack-a-thon weekend, by developer Frederik Ramm. It was written right after the hack-a-thon. Discussion continued on the mailing list as a direct result of the hack-a-thon and implementation efforts have been made.

----

We won't quite do Postgres yet, but we will say goodbye to MySQL's MyISAM tables at least (and switch over to InnoDB to achieve transactions). 

Version numbers will be added to the "current" table and returned by the API, and included in planet files. Whenever someone uploads data, they must specify the version number that their update is based on. The update is then rejected if the server has a different version. Together with transaction, this will make sure that nobody inadvertently overwrites somebody else's edit.

It will be possible to upload a large set of database modifications in one go, transactionally. So it's either everything works or everything fails. This will greatly speed-up edit uploads, as with the current scheme, it requires one connection for each object manipulation.

We will force users to group changes into "changesets" and encourage them to specify something like a "commit comment" for a changeset. Changesets will have the affected bounding box associated with them in the database, and we will provide access methods to search and list changesets.

We have not yet made plans for reverting changes.

I think we have a consensus that reverting a change should be a normal operation that in itself constitutes a change -- instead of somehow annulling a previous change in a matter-antimatter type of operation.

We do not have the pressing need to support reverts through an API call but instead can hope to outsource this to clever clients.

----

https://wiki.openstreetmap.org/wiki/%E2%82%AC15.000_Funding_for_Software_Development_-_Proposal/Bimonthly_Report_for_June_2008