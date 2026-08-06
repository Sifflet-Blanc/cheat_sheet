**[< Home](README.md)**
# Kotlin

val by default 

Never !! (prefer use ?:)

The `==` replace the `.equals()` and the `===` replace `==`


## With Spring
Remember of the config allopen because by default all classes are final in kotlin so the proxies for annotation wont work.

For the field annotation like Validation (Ex: @NotNull and @Email) I need to add @field:NotNull because otherwise the annotation will be applied to the constructor and so they won't be called at the validation time.

Don't use Lombok with Kotlin 

Use also and let 

equal hash code on the id

Don't use data class for the entity


## Coroutine 
Permit to limit the number of threads in the application. In Java we need to use one thread by call when in kotlin with coroutine only one thread will be active.


## Test
MockK for kotlin and not Mockito.
    
Like javascript kotlin permit to call the constructor only with 