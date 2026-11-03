# docs/05-donnees-persistance/donnees-distantes-api.md

RetrofitInstance: ApiClient décrit mieux ce rôle que RetrofitInstance sans laisser croire que l’objet lui-même est une instance Retrofit.

Tu pourrais donc nommer l’objet et le fichier ApiClient / ApiClient.kt, puis appeler les services ApiClient.abcApi et ApiClient.defApi. 

La version originale en ligne:

```
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object RetrofitInstance {
    private val retrofit by lazy {
        Retrofit.Builder().baseUrl("https://jsonplaceholder.typicode.com//")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }

    val apiInterface by lazy {
        retrofit.create(ApiInterface::class.java)
    }
}
```
(on demande apiInterface à RetrofitInstance, mais on pourrait demander abcApi à ApiClient, ce qui est plus clair
)

# docs/05-donnees-persistance/donnees-distantes-api-2eme-partie.md

Pas présenté dans le cours A2026 (Apps Mobiles 2 - 60h) - le faire si le cours devient un cours de 75h?