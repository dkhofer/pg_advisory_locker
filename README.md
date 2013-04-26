pg_advisory_locker
==================

Helper for calling PostgreSQL functions: pg_advisory_lock,
pg_advisory_try_lock, and pg_advisory_unlock.

Examples
========

Basic example of passing a block to the locker

```
class Foo < ActiveRecord::Base
  include PgAdvisoryLocker

  def self.lock_for_whatever(&block)
	return lock_record(0, &block)
  end

  def self.do_something
    lock_for_whatever do
      # do something here
    end
  end
end
```

Advisory lock on id:

```
class Foo < ActiveRecord::Base
  include PgAdvisoryLocker

  def lock_for_whatever
	return advisory_lock
  end

  def unlock_for_whatever
	return advisory_unlock
  end

  def do_something
    lock_for_whatever
	begin
      # do something
    ensure
	 unlock_for_whatever
    end
  end
end
```
Generic locking:
```
class Foo < ActiveRecord::Base
  include PgAdvisoryLocker
end

Foo.pg_advisory_lock(0, 0) do
  # do something
end
```
