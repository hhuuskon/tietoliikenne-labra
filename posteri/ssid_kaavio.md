```mermaid
sequenceDiagram
	participant Päätelaite
	participant Tukiasema
	Päätelaite->>Tukiasema: Kyselykehys
	Tukiasema -->>Päätelaite: Probe response
	Päätelaite->>Tukiasema: Kiinnittymispyyntö
	Päätelaite->Tukiasema: Sopivat yhdistämisen loppuun

```

