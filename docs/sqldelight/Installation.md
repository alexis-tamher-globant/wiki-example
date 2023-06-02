# Installation

1. Adding `sqldelight` dependencies on root `build.gradle` file

```groovy
// build.gradle.kts (root)
buildscript {
	...
	dependencies {
		...
		classpath("com.squareup.sqldelight:gradle-plugin:1.5.3")
	}
}
```

1. Adding `sqldelight` dependencies on shared `build.gradle` file

```groovy
// build.gradle.kts (shared)
plugins {
	id("com.squareup.sqldelight")
}
kotlin {
	...
	soursets {
		val commonMain by getting {
			dependencies {
			implementation("com.squareup.sqldelight:runtime:1.5.3")
			}
		}
		...
		val androidMain by getting {
			dependencies {
				implementation("com.squareup.sqldelight:android-driver:1.5.3")
			}
		}
		...
		val iosMain by creating {
			dependencies {
				implementation("com.squareup.sqldelight:native-driver:1.5.3")
			}
			...
		}
	}
}
```

1. Also we need to define the database on the same file with the database name and package name

```groovy
// build.gradle.kts (shared)
kotlin {
	...
}

sqldelight {
	database("NoteDataBase") {
		packageName = "com.example.notes.database"
		sourceFolders = listOf("sqldelight")
	}
}

android {
	...
}
```

1. Creating a folder into `kotlin` level called `sqldelight` like we defined on previous step. Should end up like this.

```kotlin
/shared
	/kotlin
	/sqldelight
		/database
```

1. Let's create table and queries with file extension `sq` into `/sqldelight/database`, eg `note.sq`

```sql
CREATE TABLE noteEntity(
id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
title TEXT NOT NULL,
content TEXT NOT NULL,
colorHex INTEGER NOT NULL,
created INTEGER NOT NULL
);

getAllNotes:
SELECT *
FROM noteEntity;

getNoteById:
SELECT *
FROM noteEntity
WHERE id = ?;

insertNote:
INSERT OR REPLACE
INTO noteEntity(
id,
title,
content,
colorHex,
created
) VALUES(?, ?, ?, ?, ?);

deleteNoteById:
DELETE FROM noteEntity
WHERE id = ?;
```

1. Let's sync and we'll be able to access to Entities and DataBase class we created