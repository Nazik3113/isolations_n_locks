# Isolations & locks

## POSTGRESQL

* READ UNCOMMITTED

  * Dirty Read - no
  * Nonrepeatable Read - yes
  * Phantom Read - yes
  * Lost update - yes

* READ COMMITTED

  * Dirty Read - no
  * Nonrepeatable Read - yes
  * Phantom Read - yes
  * Lost update - yes

* REPEATABLE READ

  * Dirty Read - no
  * Nonrepeatable Read - no
  * Phantom Read - no
  * Lost update - no

* SERIALIZABLE

  * Dirty Read - no
  * Nonrepeatable Read - no
  * Phantom Read - no
  * Lost update - no

## PERCONA

* READ UNCOMMITTED

  * Dirty Read - yes
  * Nonrepeatable Read - yes
  * Phantom Read - yes
  * Lost update - yes

* READ COMMITTED

  * Dirty Read - no
  * Nonrepeatable Read - yes
  * Phantom Read - yes
  * Lost update - yes

* REPEATABLE READ

  * Dirty Read - no
  * Nonrepeatable Read - no
  * Phantom Read - yes
  * Lost update - no

* SERIALIZABLE

  * Dirty Read - no
  * Nonrepeatable Read - no
  * Phantom Read - no
  * Lost update - no

## Steps to reproduce

Table: id | amount

  * Dirty Read
    ```bash
    BEGIN;

    UPDATE table SET anount = 10 WHERE id = 1;
    
    BEGIN;

    SELECT * from table where id = 1;

    COMMIT;

    END;

    COMMIT;

    END;
    ```

  * Nonrepeatable Read

    ```bash
    BEGIN;

    SELECT * from table where id = 1;

    COMMIT;

    BEGIN;

    UPDATE table SET anount = 10 WHERE id = 1;

    COMMIT;

    END;

    SELECT * from table where id = 1;

    COMMIT;

    END;
    ```

  * Phantom Read

    ```bash
    BEGIN;

    SELECT * from table amount < 100;

    COMMIT;

    BEGIN;

    INSERT INTO table (amount) values (50);

    COMMIT;

    END;

    SELECT * from table amount < 100;

    COMMIT;

    END;
    ```

  * Lost update

    ```bash
    BEGIN;

    UPDATE table SET amount = 70 where id = 1;

    BEGIN;

    UPDATE table SET amount = 30 where id = 1;

    COMMIT;

    END;

    COMMIT;

    END;
    ```
