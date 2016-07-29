# panoptes-subject-set-util
A command line utility to manage Zooniverse (Panoptes) subject sets.

## Installation
`npm install panoptes-subject-set-util --global` does the trick. The `--global` lets you run `ssutil` on the command line.

## Description
The Subject Set Utility implements a CLI to 1) list subjects sets in a project, 2) create linked lists of subjects for sequential pagination, and 3) activate/deactivate subject sets (currently only supported by Old Weather)

## Examples

### Listing Subject Sets

```
$ # list subject sets for project 195
$ ssutil list -p 195
┌──────────┬──────────┬──────────┬────────────────────┬────────────────────┐
│ ID       │ Active   │ Count    │ Short Name         │ Display Name       │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 2369     │ false    │ 11       │ some name          │ Ammen 1944         │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 2370     │ false    │ 3        │ some name          │ Ammen 1960         │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 2371     │ true     │ 10       │ Bear               │ Bear (Random)      │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 3903     │ n/a      │ 100      │ n/a                │ JS Textbook (dupl… │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 3783     │ true     │ 100      │ jsBook             │ JS Textbook (Sequ… │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 3776     │ true     │ 380      │ The Other Bear     │ Test Subject Set   │
├──────────┼──────────┼──────────┼────────────────────┼────────────────────┤
│ 3782     │ true     │ 5        │ the numbers        │ The Numbers        │
└──────────┴──────────┴──────────┴────────────────────┴────────────────────┘
```

### Creating Linked Subject Lists
```
$ ssutil link-pages -p 195 -s 3782 --cache-size 3 --dryrun
```

This generates a linked list by adding `nextSubjectIds` and `prevSubjectIds` to each subject's metadata. The `--cache-size` option determines how many next/prev subjects are stored, provided they exist. For example, the first and last subjects will have empty `prevSubjectIds` and `nextSubjectIds` arrays, respectively. The `--dryrun` option dumps an array of modified subjects and will not commit them to the server.

The code below shows the first two subjects generated by the `link-pages` command:

```
[{
		"id": "47676",
		"metadata": {
			"file": "01.jpg",
			"pageNumber": "1",
			"description": "the number one",
			"nextSubjectId": "47679",
			"prevSubjectId": null,
			"prevSubjectIds": [],
			"nextSubjectIds": ["47679", "47677", "47675"]
		},
		"locations": [{
			"image/jpeg": "https://panoptes-uploads.zooniverse.org/staging/subject_location/62774c6e-0f42-41e6-91e6-dc4b3cea24d8.jpeg"
		}],
		"zooniverse_id": null,
		"created_at": "2016-07-15T18:45:48.154Z",
		"updated_at": "2016-07-22T15:32:47.469Z",
		"href": "/subjects/47676",
		"links": {
			"project": "195"
		}
	}, {
		"id": "47679",
		"metadata": {
			"file": "02.jpg",
			"pageNumber": "2",
			"description": "the number two",
			"nextSubjectId": "47677",
			"prevSubjectId": "47676",
			"prevSubjectIds": ["47676"],
			"nextSubjectIds": ["47677", "47675", "47678"]
		},
		"locations": [{
			"image/jpeg": "https://panoptes-uploads.zooniverse.org/staging/subject_location/fa8f1237-7d78-4c72-b3a1-9d0a5c45b2e4.jpeg"
		}],
		"zooniverse_id": null,
		"created_at": "2016-07-15T18:45:49.807Z",
		"updated_at": "2016-07-22T15:32:49.067Z",
		"href": "/subjects/47679",
		"links": {
			"project": "195"
		}
	}]
```




## Requirements
1. Zooniverse acount,
2. Permissions (either as an owner or collaborator) to a Zooniverse project, and
3. Subject set linked to the project
