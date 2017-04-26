# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
A join table is valuable in order to link two tables with a many to many relationship.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    One profile has many movies
    One profile has many favorites
    One movie has many favorites

    CREATE TABLE profiles (
      id SERIAL PRIMARY KEY,
      given_name TEXT,
      surname TEXT,
      email TEXT
    );

    CREATE TABLE movies (
      id SERIAL PRIMARY KEY,
      title TEXT,
      release_date DATE,
      length INTEGER
    );

    CREATE TABLE favorites (
      id SERIAL PRIMARY KEY,
      movie_id INTEGER REFERENCES movies(id),
      profile_id INTEGER REFERENCES profiles(id)
    );
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :favorites
    has_many :movies through: :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    belongs_to :profile. :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :movies, inverse_of: :favorites
    belongs_to :profiles, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    The purpose of a serializer is to determine how many columns of data will be shown.  So a serializer with just id and name will only show id and name, even if other data points exist.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, given_name
    has_many :favorites
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorites movies:references profiles:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    Dependent destroy destroys any posts made by one resource as part of another.  If we had a list of employees and a list of tasks they performed, if we delete an employee we would want that employee's tasks to be deleted as well.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    An example of this would be an artist, a song, and a radio play resource.  One artist would have many songs, while one artist could have many radio plays and one song can also have many radio plays.
  ```
