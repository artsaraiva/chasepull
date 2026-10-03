# The product endpoint returns the whole pool, and filtering happens on the device

ChasePull is meant to be used while standing in a card shop, often on weak or unreliable mobile data. So the product endpoint returns a product's entire pool in one response: every variant, already grouped into cards and treatment families, with tags, prices and odds. A large pool such as LCI Collector, about 600 variants, is roughly 60–100 KB gzipped. Filtering and sorting then run on the device through one shared, pure module, so every filter tap is instant and needs no network.

We considered server-side pagination and filtering. It was rejected, because each filter tap would cost a network round trip at exactly the moment the connection is worst. Grouping and chase ranking stay on the server so that web and native clients agree. Revisit this only if pool payloads grow past what a phone downloads comfortably.
