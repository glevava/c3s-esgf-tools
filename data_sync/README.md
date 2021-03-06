# data sync tools

To use (for cp4cds):

 * edit `vars_cp4cds` to set:
    * `local_data_root` : where you want to sync the data to
    * `myproxy_username`

 * run `do_all.sh cp4cds`

----
This will:

* maintain _two_ local mapfile directory trees; one ("orig") is a verbatim copy of the remote mapfiles, the other ("edited") has the data file paths edited to reflect the local paths, and can be used for publication

Each time:
* do a sync of the "orig" mapfiles directory from the remote in order to obtain any new mapfiles
* compare the "orig" and "edited" mapfiles directories to see which mapfiles were downloaded for which a corresponding edited mapfile does not yet exist
* for each of these, create the edited mapfile. Also create a list of all the newly fetched mapfiles.
* use the list of newly fetched mapfiles to create a list of new datafiles which need to be fetched - these will be datafiles that do not already exist, or for which the size listed in the mapfile does not match the size of the existing file (note that the checksum is not used). In normal operation, the data file would not already exist, but this might not be the case where --rebuild is used -- see below.
* fetch these new datafiles


For a dry run:

  * do `do_all.sh --dry-run cp4cds`
  * This will use copies of the mapfiles under `/tmp/mapfiles` (or `$TMPDIR/mapfiles`).
  * The relevant lists will be created with a `.DRYRUN` suffix, and the new data files will not be downloaded.



"Rebuild" option
  * do `do_all.sh --rebuild cp4cds`  (this can be combined with `--dry-run`)
  * This will recreate _all_ the edited mapfiles, not only the ones which did not exist previously.
  * Because it is tested whether an existing data file exists with the size that is specified in the mapfile, the result will be to fetch only the new (or not fully downloaded) data files, even though all mapfiles are processed.


