# Neo4j Lab

## Running the container

Run the following command to start working with Neo4j:

    docker-compose up -d

## Import data from csv file

Run the following command to import data from csv:

    LOAD CSV WITH HEADERS
    FROM 'file:///people.csv'
    as row
    MERGE (p: Person{personId: row.personId, name: row.name, birthYear: row.birthYear})
    RETURN p

Cypher query for persons.csv:

    LOAD CSV WITH HEADERS
    FROM 'file:///persons.csv' AS row
    MERGE (p:Person {tmdbId: toInteger(row.person_tmdbId)})
    SET
    p.imdbId = toInteger(row.person_imdbId),
    p.bornIn = row.bornIn,
    p.name = row.name,
    p.bio = row.bio,
    p.poster = row.poster,
    p.url = row.url,
    p.born = row.born,
    p.died = row.died

Create movies nodes:

    LOAD CSV WITH HEADERS
    FROM 'file:///movies.csv' AS row
    MERGE (m:Movie {movieId: toInteger(row.movieId)})
    SET
    m.tmdbId = row.movie_tmdbId,
    m.imdbId = row.movie_imdbId,
    m.released = row.released,
    m.title = row.title,
    m.year = toInteger(row.year),
    m.plot = row.plot,
    m.budget = toFloat(row.budget)

Load csv and create nodes with their relationships:

    LOAD CSV WITH HEADERS
    FROM 'file:///acted_in.csv' AS row
    MATCH (p:Person {tmdbId: toInteger(row.person_tmdbId)})
    MATCH (m:Movie {movieId: toInteger(row.movieId)})
    MERGE (p)-[r:ACTED_IN]->(m)
    SET r.role = row.role

## Create constraints and indexes

Run the following command to create an unique value for given attribute:

    CREATE CONSTRAINT attribute_name_unique IF NOT EXISTS
    FOR (l: Label)
    REQUIRE l.attribute_name IS UNIQUE;

Create an index:

    CREATE INDEX index_name FOR (n:Label) ON (n.property)
