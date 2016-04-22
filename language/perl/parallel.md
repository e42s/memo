# perlで並列処理
```perl
use Parallel::ForkManager;

my $pm = Parallel::ForkManager->new(8);

while(1 == 1){
  $pm->start and next;
  ...
  $pm->finish;
}
$pm->wait_all_children;
```
