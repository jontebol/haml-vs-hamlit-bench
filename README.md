This was created in order to benchmark some templates with more classes (like with typical Tailwind use), to see which templating gem would be faster for our app.

Here are the results for me locally. Note that I've made changes to the benchmark script (+ templates) which may skew the results, so take this with a big grain of salt until verified by someone who knows these gems better. 

**UPDATE:** I'm happy to report that with some changes to the benchmarks script that made the comparison accurate (haml v5.2.0 wasn't escaping html while the other gems were), and a quick little [fix](https://github.com/k0kubun/hamlit/commit/bc5484f5f342e36d6978c0217b0cc48703529be7) from [@k0kubun](https://github.com/k0kubun), hamlit is now coming out on top as expected.

```
$ bundle exec ruby -v benchmark/slim/run-benchmarks.rb
ruby 2.6.8p205 (2021-07-07 revision 67951) [arm64-darwin21]
Calculating -------------------------------------
       erubi v1.11.0    29.191k i/100ms
         slim v4.1.0    35.388k i/100ms
       hamlit v3.0.2    36.976k i/100ms
         haml v5.2.0    29.621k i/100ms
-------------------------------------------------
       erubi v1.11.0    310.588k (± 0.6%) i/s -      1.576M
         slim v4.1.0    383.155k (± 0.4%) i/s -      1.946M
       hamlit v3.0.2    401.604k (± 1.0%) i/s -      2.034M
         haml v5.2.0    312.471k (± 1.7%) i/s -      1.570M

Comparison:
       hamlit v3.0.2:   401604.5 i/s
         slim v4.1.0:   383155.0 i/s - 1.05x slower
         haml v5.2.0:   312471.4 i/s - 1.29x slower
       erubi v1.11.0:   310587.9 i/s - 1.29x slower
```

For fun, I also tested with Ruby 3.2.1 which gave quite a nice speedup:

```
$ bundle exec ruby -v benchmark/slim/run-benchmarks.rb
ruby 3.2.1 (2023-02-08 revision 31819e82c8) [arm64-darwin21]
Calculating -------------------------------------
       erubi v1.11.0    38.111k i/100ms
         slim v4.1.0    39.468k i/100ms
       hamlit v3.0.2    41.777k i/100ms
         haml v5.2.0    28.209k i/100ms
-------------------------------------------------
       erubi v1.11.0    408.614k (± 1.6%) i/s -      2.058M
         slim v4.1.0    445.191k (± 1.2%) i/s -      2.250M
       hamlit v3.0.2    464.592k (± 1.2%) i/s -      2.340M
         haml v5.2.0    293.733k (± 1.5%) i/s -      1.495M

Comparison:
       hamlit v3.0.2:   464591.8 i/s
         slim v4.1.0:   445190.9 i/s - 1.04x slower
       erubi v1.11.0:   408614.1 i/s - 1.14x slower
         haml v5.2.0:   293733.5 i/s - 1.58x slower
```

Old results using hamlit 3.0.1:

```
$ bundle exec ruby -v benchmark/slim/run-benchmarks.rb 
ruby 2.6.8p205 (2021-07-07 revision 67951) [arm64-darwin21]
Calculating -------------------------------------
       erubi v1.11.0    29.300k i/100ms
         slim v4.1.0    35.361k i/100ms
       hamlit v3.0.1    31.447k i/100ms
         haml v5.2.0    37.191k i/100ms
-------------------------------------------------
       erubi v1.11.0    309.763k (± 0.2%) i/s -      1.553M
         slim v4.1.0    380.053k (± 0.4%) i/s -      1.909M
       hamlit v3.0.1    335.534k (± 0.1%) i/s -      1.698M
         haml v5.2.0    395.905k (± 1.3%) i/s -      2.008M

Comparison:
         haml v5.2.0:   395905.4 i/s
         slim v4.1.0:   380052.9 i/s - 1.04x slower
       hamlit v3.0.1:   335533.8 i/s - 1.18x slower
       erubi v1.11.0:   309763.0 i/s - 1.28x slower
```
