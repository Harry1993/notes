# Deep Notes

## Tensorflow

### Get logits values

We can at first virtualize the graph (`.pb` file) to find the name of the
interesting node by running the following Python script
(`~/workspace/deep.learning/models/pb_viewer.py`) and then executing
`tensorboard --logdir path/to/logs`.

```
import tensorflow as tf
from tensorflow.python.platform import gfile
with tf.Session() as sess:
    model_filename ='./classify_image_graph_def.pb'
    with gfile.FastGFile(model_filename, 'rb') as f:
        graph_def = tf.GraphDef()
        graph_def.ParseFromString(f.read())
        g_in = tf.import_graph_def(graph_def)
LOGDIR='./graphs'
train_writer = tf.summary.FileWriter(LOGDIR)
train_writer.add_graph(sess.graph)
train_writer.close()
```

Now, we know the logits value is in the softmax node. Then the name of the
subnode is `softmax/logits:0`, which is plugged in the following lines of code
(in `tensorflow/models/tutorials/image/imagenet/classify_image.py`)

```
    softmax_tensor = sess.graph.get_tensor_by_name('softmax:0')
    logits_tensor = sess.graph.get_tensor_by_name('softmax/logits:0')
    logits, predictions = sess.run([logits_tensor, softmax_tensor],
                           {'DecodeJpeg/contents:0': image_data})
    print('logits: max:', logits.max(), 'min:', logits.min())
```

## String

*	

## Misc
