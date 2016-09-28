# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  # < Your Response Here >
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
Profiles--Favorites--Movies

A profile can have many favorites and a movie can have many profiles through
favorites.

```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites, dependent: :destroy
end
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites, dependent: :destroy
end
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs to :profile, inverse_of: :favorites
  belongs to :movie, inverse_of :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  A serializer provides a way to create a custom view of the data you want to show from
  specific tables.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :given_name, :surname, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
bundle exec rails g scaffold Favorites user:string movie_name:string profile:references movie:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  Dependent: Destroy is used to delete an association with tables. For instance,
  if a patient moved, we would want to delete their record, along with any
  appointments that were associated with that patient. You would add dependent: Destroy
  in the model of the patient

  class Patient < ActiveRecord::Base
  has_many :doctors, through: :appointments
  has_many :appointments, dependent: :destroy
end
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
 One to many:  An author can have many books.
 Many to many: Books can have many borrowers and borrowers can have many books
 through loans.
```
