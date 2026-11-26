# BenchmarkDotNet. Один контракт Gate

Сохранённый локальный прогон от 24 ноября 2026 года. В отчёте указаны окружение и параметры. Точная ревизия исходников в исходном отчёте не зафиксирована.

Сценарии и фикстура доступны на коммите `203e1e8527d06352d42a5ac93ca95e723f62c140` демонстрационного репозитория. `fixtures/gate.contract.json` в этой ревизии содержит 1 312 байт UTF-8 и 49 ключей. SHA-256 `482df5a890af627824cff5f6fffff2c92179dad50860025dfbc62d9c16c8b88f`. [Сценарии](https://github.com/mzaprudin/zero-alloc-json/blob/203e1e8527d06352d42a5ac93ca95e723f62c140/benchmarks/ZeroAlloc.Json.Benchmarks/LargeJsonParsingBenchmarks.cs) · [Фикстура](https://github.com/mzaprudin/zero-alloc-json/blob/203e1e8527d06352d42a5ac93ca95e723f62c140/fixtures/gate.contract.json)

Чтобы повторить сценарии с параметрами этого отчёта, из корня демо на указанной ревизии выполняется команда с .NET SDK 10.0.115 и runtime 10.0.15.

```sh
dotnet run -c Release --project benchmarks/ZeroAlloc.Json.Benchmarks -- \
  --filter '*.LargeJsonParsingBenchmarks.*' \
  --warmupCount 3 --iterationCount 10 --iterationTime 300 --launchCount 1 --exporters json
```

Прочерк в колонке `Allocated` означает 0 B/op.

```text

BenchmarkDotNet v0.15.8, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD Ryzen 9 5950X 0.55GHz, 1 CPU, 32 logical and 16 physical cores
.NET SDK 10.0.115
  [Host]     : .NET 10.0.15 (10.0.15, 10.0.1526.53008), X64 RyuJIT x86-64-v3
  Job-KOVDBU : .NET 10.0.15 (10.0.15, 10.0.1526.53008), X64 RyuJIT x86-64-v3

IterationCount=15  IterationTime=500ms  LaunchCount=1  
WarmupCount=5  

```
| Method                  | Mean     | Error     | StdDev    | Ratio | Gen0   | Allocated | Alloc Ratio |
|------------------------ |---------:|----------:|----------:|------:|-------:|----------:|------------:|
| SystemTextJson          | 2.301 μs | 0.0250 μs | 0.0234 μs |  1.00 | 0.0091 |     208 B |        1.00 |
| PopulatorNewObject      | 1.978 μs | 0.0142 μs | 0.0126 μs |  0.86 | 0.0120 |     208 B |        1.00 |
| PopulatorReusedObject   | 1.945 μs | 0.0128 μs | 0.0119 μs |  0.85 | 0.0039 |     120 B |        0.58 |
| PopulatorReusedInterned | 1.857 μs | 0.0096 μs | 0.0080 μs |  0.81 |      - |         - |        0.00 |
| PopulatorStructRef      | 1.833 μs | 0.0109 μs | 0.0097 μs |  0.80 |      - |         - |        0.00 |
