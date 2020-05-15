# Horse-Paginate
Middleware para Servidor Horse para Paginação de Dados JSON

Para Habilitar a Paginação de Dados JSON com a requisição vinda do cliente 
é necessário passar o seguinte parametro do HEADER da Requisição

X-Paginate = true;

Para controlar a paginação você pode enviar os seguintes parametros na URL da sua requisição

limit=X (Esse parametro define o limite de registros da paginação)

page=X (Esse parametro informa qual a pagina da paginação deve ser retornada)

Exemplo

www.seusite.com:9000/ping?limit=10&page=5


Sample Horse Server
```delphi

uses
  System.SysUtils,
  Horse,
  Horse.Paginate,
  Horse.Jhonson,
  System.JSON,
  DBClient,
  DataSet.Serialize;

var
  App: THorse;

begin
  App := THorse.Create(9000);
  App.Use(Paginate);
  App.Use(Jhonson);

  App.Get('/ping',
  procedure(Req: THorseRequest; Res: THorseResponse; Next: TProc)
  var
    DataSet : TClientDataSet;
  begin
    DataSet := TClientDataSet.Create(nil);
    try
      DataSet.LoadFromFile('items.xml');
      Res.Send<TJsonArray>(DataSet.ToJsonArray);
    finally
      DataSet.Free;
    end;

  end);

  App.Start;
end.
```
