# cloudnative-vectorchord

> [!WARNING]
> The whole purpose of this image is to provide a way to migrate from vecto.rs to VectorChord.
> It was only made to migrate an existing [immich](https://github.com/immich-app/immich) database using vecto.rs.



Container images for [cloudnative-pg](https://cloudnative-pg.io/) with [pgvecto.rs](https://github.com/tensorchord/pgvecto.rs) and [VectorChord](https://github.com/tensorchord/VectorChord) extensions installed.

## Example 

From pgvecto.rs 0.3.0

```diff
apiVersion: postgresql.cnpg.io/v1
 kind: Cluster
 spec:
   (...)
-  imageName: ghcr.io/tensorchord/cloudnative-pgvecto.rs:16.5-v0.3.0@sha256:be3f025d79aa1b747817f478e07e71be43236e14d00d8a9eb3914146245035ba
+  imageName: ghcr.io/duvholt/cloudnative-vectorchord-pgvecto.rs:16-0.3.0-v0.3.0@sha256:0456bb83c28ce3f3815b7ae5e2dfc5e21c3483b13ed30af4d6c46e56808e0b06
   postgresql:
     shared_preload_libraries:
       - "vectors.so"
+      - "vchord.so"
   bootstrap:
     initdb:
       postInitSQL:
         - ALTER SYSTEM SET search_path TO "$user", public, vectors;
         - CREATE EXTENSION IF NOT EXISTS "vectors";
+        - CREATE EXTENSION IF NOT EXISTS vchord CASCADE;
```

## Building

To build the Dockerfile locally, you need to pass the `CNPG_TAG`, `PGVECTORS_TAG` and `VECTORCHORD_TAG` args. For example:  
`docker build . --build-arg="CNPG_TAG=16.9" --build-arg="PGVECTORS_TAG=0.3.0" --build-arg="VECTORCHORD_TAG=0.3.0"` 
