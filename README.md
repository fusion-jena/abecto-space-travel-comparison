# Space Travel Data Comparison of Space Travel Data in Wikidata and DBpedia with ABECTO

This is a project to compare space travel data from Wikidata and DBpedia using [ABECTO](https://github.com/fusion-jena/abecto).

## Execution

The comparison pipeline is automatically executed on each push using GitHub Actions.

To running the project locally, you first need to checkout ABECTO and this project and to build ABECTO:
```
git clone git@github.com:fusion-jena/abecto.git
mvn -f abecto -B -Dmaven.test.skip=true package
git clone https://github.com/fusion-jena/abecto-space-travel-comparison.git
java -jar abecto/target/abecto.jar --trig abecto-space-travel-comparison/space-travel-comparison-result.trig abecto-space-travel-comparison/space-travel-comparison.trig
```
