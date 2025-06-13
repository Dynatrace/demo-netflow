It is time to view the data in Dynatrace.

--8<-- "snippets/bizevent-view-data.js"

!!! info "Data Availability"
    It may take up to 1 minute for data to appear in Dynatrace. Be patient and re-execute the command a few times.
    
In Dynatrace:

* Press `ctrl + k` search for `notebooks`
* Add a new section for `DQL`
* Search for:

```{ "name": "fetch log line" }
fetch logs
| filter exists(content, "Log line")
```

![dynatrace notebook netflow](images/dt-notebook-1.png)


## Congratulations

You have succesfully ingested NetFlow data into Dynatrace.

This Observability lab is now complete.

<div class="grid cards" markdown>
- [Click here to cleanup your environment :octicons-arrow-right-24:](cleanup.md)
</div>
