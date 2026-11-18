I just had my first open-source pull request merged into Nimbus, making Zotero reading notes searchable alongside paper references. Notes stay indexed locally on your machine rather than being sent to a remote AI service.

Previously, Nimbus's Zotero connector skipped all notes, so searching your library never surfaced anything you had written about a paper. I claimed the issue, wrote a dedicated note mapper that turns note HTML into plain text with a readable title, and routed notes to Nimbus's local-only embedding path so personal notes stay private.

After an automated code reviewer flagged two edge cases—inline formatting tags splitting words like H2O and numeric HTML characters staying encoded—the maintainer, asafgolombek, reviewed my scoping, pushed those final fixes directly to my branch so we wouldn't need another round trip, and merged the PR.

Working on this taught me to check a project's privacy and data-routing rules before writing a single line of code, and to test messy formatting edge cases like inline HTML before opening a pull request.

https://github.com/nimbus-agent/Nimbus/pull/1965

#OpenSource #TypeScript #SoftwareEngineering #AI #Zotero
