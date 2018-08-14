# global-timer
Distributed timer for workerman.

# Examples
```php
<?php
require __DIR__ . '/../vendor/autoload.php';
use Workerman\Worker;
use Workerman\GlobalTimer;

$global_channel_server = new Channel\Server('0.0.0.0', 3333);

$worker = new Worker('text://0.0.0.0:2222');
$worker->count = 4;
$worker->onWorkerStart = function($worker){
    GlobalTimer::init('127.0.0.1','3333');
    if ($worker->id == 0 ) {
        $timer_id = GlobalTimer::add(1, function() use (&$timer_id){
            echo "worker[0] tick timer_id:$timer_id\n";
        });
    }
};
$worker->onMessage = function($con, $data) use ($worker){
    $timer_id = $data;
    echo "worker[".$worker->id."] del timer_id:$timer_id\n";
    GlobalTimer::del($timer_id);
};
Worker::runAll();
```
