# sumcolumns
Replace the values of tables, columns and parameters with your own.

To call the supabase function via rpc use this code in a custom action

```
Future<double?> functionName(
  String paramater1,
  String paramater2,
) async {
  try {
    final supabase = SupaFlow.client;
    final response = await supabase
        .rpc('sqlFunction', params: {'p_var1': paramater1, 'p_var2': paramater2});
    // Print the raw response for debugging purposes
    print('Raw Response: $response');

    // Return the raw response
    return response; // Assuming response is of type double?
  } catch (e) {
    print('Exception:$e');
  }
}

```

In your Supabase functions use this query to filter and sum the columns. 

```
CREATE
OR REPLACE FUNCTION public.sqlFunction("p_var1" TEXT, "p_var2" TEXT) RETURNS NUMERIC AS $$
DECLARE
    total_sum NUMERIC;

BEGIN

    SELECT
               SUM(("yourTable"."columnName")::NUMERIC) INTO total_sum
    FROM
        "yourTable"
    WHERE
        "yourTable".option1 = p_var1 :: uuid -- Explicitly cast p_userid to UUID
        AND "UserData".option2 = p_var2
        // AND add more as required; 
   

    RETURN COALESCE(total_sum, 0);

END;
$$ LANGUAGE plpgsql;

```
