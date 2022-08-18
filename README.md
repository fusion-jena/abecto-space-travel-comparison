# Comparison of Space Travel Data in Wikidata and DBpedia with ABECTO

This is a project to compare space travel data from [Wikidata](https://www.wikidata.org) and [DBpedia](https://www.dbpedia.org) using [ABECTO](https://github.com/fusion-jena/abecto) and generate a report for the [Wikidata Mismatch Finder](https://www.wikidata.org/wiki/Wikidata:Mismatch_Finder).

## Execution on GitHub Actions

There is a GitHub Actions Workflow configured in [.github/workflows/space-travel-comparison.yml](.github/workflows/space-travel-comparison.yml) that can be manually started on [Actions > Space Travel Comparison](https://github.com/jmkeil/abecto-space-travel-comparison/actions/workflows/space-travel-comparison.yml) by users with sufficient privileges. (You might fork this repository, to try it.) If a token was provided, the results will get uploaded to the Wikidata Mismatch Finder.

## Execution on Your Machine

To running the project on your machine, please take the following steps:

1. clone and build ABECTO

	```
	git clone --depth 1 -b v1.0.1 git@github.com:fusion-jena/abecto.git
	mvn -f abecto -B -Dmaven.test.skip=true package
	```

2. clone this project

	```
	git clone git@github.com:jmkeil/abecto-space-travel-comparison.git
	```

3. execute the comparison pipeline defined in [space-travel-comparison.trig](space-travel-comparison.trig)

	```
	java -jar abecto/target/abecto.jar --trig abecto-space-travel-comparison/space-travel-comparison-result.trig abecto-space-travel-comparison/space-travel-comparison.trig
	```

4. create a Wikidata Mismatch Finder report

	```
	java -jar abecto/target/abecto.jar --loadOnly --export wdMismatchFinder=abecto-space-travel-comparison/space-travel-comparison-wdMismatchFinder.csv abecto-space-travel-comparison/space-travel-comparison-result.trig
	```

5. upload the report (replace `<TOKEN>` with your [personal access token](https://github.com/wmde/wikidata-mismatch-finder/blob/main/docs/UserGuide.md#obtaining-an-api-access-token-))

	```
	curl -X POST "https://mismatch-finder.toolforge.org/api/imports" \
	  -H "Accept: application/json" \
	  -H "Authorization: Bearer <TOKEN>" \
	  -F "mismatch_file=@abecto-space-travel-comparison/space-travel-comparison-wdMismatchFinder.csv " \
	  -F "description=Mismatches found by a comparison of space travel data from Wikidata and DBpedia using ABECTO. See: https://github.com/jmkeil/abecto-space-travel-comparison" \
	  -F "external_source=DBpedia" \
	  -F "external_source_url=http://dbpedia.org/"
	```
