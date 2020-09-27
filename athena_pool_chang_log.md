# athena_pool changelog

# 0.1.0 / 2020-09-27

给以下表增加若干字段，方便业务处理：
- add signature and size_bytes to blocks table：signature，size_bytes  c5d96b9234bfca1894d1a0e64f1ffb7f3fc351ce
- add version to messages table：version 0cf84351c0c2ec3130909861b2f9f65f6496a38e

以下表与业务无关暂时取消：
- comment table related market： market_deal_proposals, market_deal_states f326f9b974e48adebf477aad271192a7e6c237ca
- comment table related deal：minerid_dealid_sectorid 02e34d06e151a38d86ca7bcdae7a845c11507c83
- comment table related drand： drand_entries， block_drand_entries 28785d698d8ac16b5179745979c90240bbf4f875
 
以下是官方代码没有完成，或者处理时间过长，暂时注释掉。后期得回复回来 
- comment codes related panic("TODO") : miner_sector_events 20becf7636de40980acce284d295c4002e7a2891
- comment table related sector : sector_info, sector_precommit_info 293ccec018aa42ca7a0a675bb6d48b9ab144702e
  
防止程序空转，暂时注释掉，数据同步完了再打开  
- comment Syncer and Scheduler 2516ebc40c5d8dc32f42294fc93de111e1e27fc0

以下为优化的部分，串行处理改成并行处理。
- optimize unprocessedBlocks to parallel execution 17fa73396bf8f7db7d8730f32205ce63c688ea13
- optimize processMiners to parallel calls f6193be14179467895b9b1cd1f9c9daaabbe3ea9
- add fetchMinersActorInfoStates for parallel calls when storeMinersAct… 63b77d1c7148e718b7fe1c7eaeb3c2fa5202845a
  

以下是优化日志，可略过：

```bash
2020-09-26T21:46:25.527+0800    DEBUG   processor       processor/processor.go:226      Collected Actor Changes {"duration": "9.175796348s"} 已经优化
2020-09-26T21:46:54.257+0800    DEBUG   processor       processor/miner.go:1031 Stored Miners Power     {"duration": "27.168437936s"}  无法优化
2020-09-26T21:47:05.571+0800    DEBUG   processor       processor/common_actors.go:138  Stored Actor Addresses  {"duration": "40.043644237s"} 无法优化
2020-09-26T21:47:23.605+0800    DEBUG   processor       processor/common_actors.go:214  Stored Actor Heads      {"duration": "18.034519407s"} 无法优化
2020-09-26T21:47:53.443+0800    DEBUG   processor       processor/miner.go:940  Stored Miners Actor State       {"duration": "1m26.354367748s"} 已经优化
2020-09-26T21:47:53.444+0800    DEBUG   processor       processor/messages.go:98        Handled Message Changes {"duration": "1m27.916465029s"} 已经优化
2020-09-26T21:47:53.471+0800    DEBUG   processor       processor/common_actors.go:260  Stored Actor States     {"duration": "47.900256425s"} 无法优化
2020-09-26T21:49:14.800+0800    DEBUG   processor       processor/miner.go:321  Stored Miner Pre Commit Info    {"duration": "2m47.711262443s"} 已经优化,可暂时删除表
2020-09-26T21:50:16.434+0800    DEBUG   processor       processor/miner.go:457  Stored Miner Sector Info        {"duration": "3m49.345693506s"} 已经优化,可暂时删除表



2020-09-26T17:06:57.343+0800    DEBUG   processor       processor/processor.go:398      Gathered Blocks to process      {"duration": "6.0615023s"} 已经优化
2020-09-26T17:07:13.836+0800    DEBUG   processor       processor/processor.go:226      Collected Actor Changes {"duration": "16.493315279s"} 已经优化
2020-09-26T17:07:15.526+0800    DEBUG   processor       processor/miner.go:210  Processed Miners        {"duration": "1.689778063s"} 已经优化
2020-09-26T17:07:24.826+0800    DEBUG   processor       processor/miner.go:1031 Stored Miners Power     {"duration": "9.299402465s"} 无法优化
2020-09-26T17:07:34.681+0800    DEBUG   processor       processor/miner.go:940  Stored Miners Actor State       {"duration": "19.155287546s"} 已经优化
2020-09-26T17:08:05.057+0800    DEBUG   processor       processor/common_actors.go:138  Stored Actor Addresses  {"duration": "51.220290257s"} 无法优化
2020-09-26T17:08:47.716+0800    DEBUG   processor       processor/common_actors.go:214  Stored Actor Heads      {"duration": "42.659460414s"} 无法优化
2020-09-26T17:08:54.224+0800    DEBUG   processor       processor/common_actors.go:260  Stored Actor States     {"duration": "49.167144856s"} 无法优化
2020-09-26T17:09:02.258+0800    DEBUG   processor       processor/messages.go:98        Handled Message Changes {"duration": "1m48.422113499s"} 已经优化
2020-09-26T17:10:09.291+0800    DEBUG   processor       processor/miner.go:321  Stored Miner Pre Commit Info    {"duration": "2m53.764969129s"} 已经优化
2020-09-26T17:11:03.462+0800    DEBUG   processor       processor/miner.go:457  Stored Miner Sector Info        {"duration": "3m47.935396414s"} 已经优化




2020-09-26T10:48:03.458+0800    DEBUG   processor       processor/processor.go:398      Gathered Blocks to process      {"duration": "10.073314361s"} 已经优化
2020-09-26T10:48:10.717+0800    DEBUG   processor       processor/processor.go:226      Collected Actor Changes {"duration": "7.259032442s"} 已经优化
2020-09-26T10:48:14.803+0800    DEBUG   processor       processor/miner.go:940  Stored Miners Actor State       {"duration": "3.888805931s"} 已经优化
2020-09-26T10:48:16.404+0800    DEBUG   processor       processor/messages.go:98        Handled Message Changes {"duration": "5.686635122s"} 已经优化
2020-09-26T10:48:23.920+0800    DEBUG   processor       processor/miner.go:321  Stored Miner Pre Commit Info    {"duration": "13.005004294s"} 已经优化
2020-09-26T10:48:26.144+0800    DEBUG   processor       processor/common_actors.go:138  Stored Actor Addresses  {"duration": "15.42634169s"}  暂时无法优化
2020-09-26T10:48:26.932+0800    DEBUG   processor       processor/common_actors.go:104  Handled CommonActors Changes    {"duration": "16.214321914s"} 已经优化, 可删除日志
2020-09-26T10:48:29.726+0800    DEBUG   processor       processor/miner.go:457  Stored Miner Sector Info        {"duration": "18.811674726s"} 已经优化 
2020-09-26T10:48:29.726+0800    DEBUG   processor       processor/miner.go:264  Persisted Miners        {"duration": "18.811769525s"}   已经优化, 可删除日志
2020-09-26T10:48:29.726+0800    DEBUG   processor       processor/miner.go:192  Handled Miner Changes   {"duration": "19.009022895s"}   已经优化, 可删除日志




2020-09-26T08:31:48.106+0800    DEBUG   processor       processor/processor.go:398      Gathered Blocks to process      {"duration": "11.903876131s"}  已经优化
2020-09-26T08:31:55.101+0800    DEBUG   processor       processor/miner.go:1015 Stored Miners Power     {"duration": "5.841592954s"}   数据量大。无法优化
2020-09-26T08:31:55.471+0800    DEBUG   processor       processor/miner.go:924  Stored Miners Actor State       {"duration": "6.211422438s"}  已经优化
2020-09-26T08:32:01.502+0800    DEBUG   processor       processor/common_actors.go:133  Stored Actor Addresses  {"duration": "12.382012623s"}   暂时无法优化
2020-09-26T08:32:02.833+0800    DEBUG   processor       processor/common_actors.go:255  Stored Actor States     {"duration": "1.330480617s"}    暂时无法优化
2020-09-26T08:32:03.440+0800    DEBUG   processor       processor/common_actors.go:209  Stored Actor Heads      {"duration": "1.938153416s"}    暂时无法优化


2020-09-25T14:51:32.690+0800    DEBUG   processor       processor/processor.go:333      Gathered Blocks to process      {"duration": "5.932971838s"}   串行改并行
2020-09-25T14:51:35.632+0800    DEBUG   processor       processor/processor.go:226      Collected Actor Changes {"duration": "2.942776445s"}   增加并行数
2020-09-25T14:51:39.655+0800    DEBUG   processor       processor/miner.go:202  Processed Miners        {"duration": "4.022065371s"}   串行改并行
2020-09-25T14:51:42.912+0800    DEBUG   processor       processor/miner.go:887  Stored Miners Actor State       {"duration": "3.257164545s"}   串行改并行
2020-09-25T14:51:44.070+0800    DEBUG   processor       processor/common_actors.go:133  Stored Actor Addresses  {"duration": "8.437381674s"}   暂时无法优化

```

