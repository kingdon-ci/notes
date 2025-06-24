* [ ] Order more house air filters for Furnace/AC
* [ ] Get the furnace serviced
* [ ] Finish migrating from NETMOOM to METNOOM
	* [ ] plex
	* [ ] bumbu
	* [ ] file shares
	* [ ] set up DNS (pihole) with Terraform or something
		* [ ] set up Primary/Secondary DNS with common data source
		* [ ] set up Split Horizon DNS - for metnoom and netmoom
	* [ ] set up second network interface on metnoom (for 12net)
	* [ ] investigate what happens when all infra is reset
		* [ ] (we found that vclusters were non-responsive with an authentication error)
		      and needed to be reset, can we detect that condition and automate it
* [ ] 