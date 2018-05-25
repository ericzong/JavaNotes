# 函数式接口

| 接口                        | 描述                    |
| --------------------------- | ----------------------- |
| BiConsumer<T,U>         | (T, U)                  |
| BiFunction<T,U,R>       | R (T, U)                |
| BinaryOperator\<T>      | T (T)                   |
| BiPredicate<T,U>        | boolean (T, U)          |
| BooleanSupplier         | boolean ()              |
| Consumer\<T>            | (T)                     |
| DoubleBinaryOperator    | double (double, double) |
| DoubleConsumer          | (double)                |
| DoubleFunction\<R>      | R (double)              |
| DoublePredicate         | boolean (double)        |
| DoubleSupplier          | double ()               |
| DoubleToIntFunction     | int (double)            |
| DoubleToLongFunction    | long (double)           |
| DoubleUnaryOperator     | double (double)         |
| Function<T,R>           | R (T)                   |
| IntBinaryOperator       | int (int, int)          |
| IntConsumer             | (int)                   |
| IntFunction\<R>         | R (int)                 |
| IntPredicate            | boolean (int)           |
| IntSupplier             | int ()                  |
| IntToDoubleFunction     | double (int)            |
| IntToLongFunction       | long (int)              |
| IntUnaryOperator        | int (int)               |
| LongBinaryOperator      | long (long, long)       |
| LongConsumer            | (long)                  |
| LongFunction\<R>        | R (long)                |
| LongPredicate           | boolean (long)          |
| LongSupplier            | long ()                 |
| LongToDoubleFunction    | double (long)           |
| LongToIntFunction       | int (long)              |
| LongUnaryOperator       | long (long)             |
| ObjDoubleConsumer\<T>   | (T, double)             |
| ObjIntConsumer\<T>      | (T, int)                |
| ObjLongConsumer\<T>     | (T, long)               |
| Predicate\<T>           | boolean (T)             |
| Supplier\<T>            | T ()                    |
| ToDoubleBiFunction<T,U> | double (T, U)           |
| ToDoubleFunction\<T>    | double (T)              |
| ToIntBiFunction<T,U>    | int (T, U)              |
| ToIntFunction\<T>       | int (T)                 |
| ToLongBiFunction<T,U>   | long (T, U)             |
| ToLongFunction\<T>      | long (T)                |
| UnaryOperator\<T>       | T (T)                   |

