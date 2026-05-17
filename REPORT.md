# Decision Transformer Memory Comparison

Task: `velocity_cartpole`. The observation hides velocity, so the problem is partially observable.

Dataset quality was high: 100 trajectories, 49,886 total steps, mean episode reward 498.86, median 500.00, min/max 475.00/500.00.

## Results

| Model | Best validation result | First strong epoch | Final evaluation | Outcome |
| --- | ---: | ---: | ---: | --- |
| DT, no memory | 162.50 return, 20% success | epoch 2 | 112.40 return, 10% success | unstable |
| DT + GRU | 500.00 return, 100% success | epoch 2 | 500.00 return, 100% success | solved |
| DT + LSTM | 500.00 return, 100% success | epoch 2 | 500.00 return, 100% success | solved |

Training loss comparison:

| Model | Initial train loss | Final train loss | Final val loss |
| --- | ---: | ---: | ---: |
| DT, no memory | 0.4768 | 0.3624 | 0.3583 |
| DT + GRU | 0.4822 | 0.3512 | 0.3516 |
| DT + LSTM | 0.4844 | 0.3512 | 0.3520 |

## Comparison

The baseline DT reduced the supervised loss, but it did not learn a reliable policy. Its validation performance was noisy: it reached 162.50 mean return at epoch 2, then dropped sharply at epoch 3, and final evaluation was only 112.40.

Both memory models solved the task. GRU reached 100% validation success by epoch 2. LSTM was even stronger after epoch 1, with 454.90 mean return and 90% success, and also reached 100% success by epoch 2.

Final performance of GRU and LSTM was identical in this run. The main difference is early training: LSTM learned a good policy faster in the first epoch, while GRU caught up by the second epoch.

## Conclusion

Adding explicit memory greatly helps the decision transformer.

For this experiment, both GRU and LSTM are sufficient. LSTM has slightly better early convergence, but GRU achieves the same final result with a simpler recurrent unit.
