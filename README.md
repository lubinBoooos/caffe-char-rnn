# caffe-char-rnn

This code is an adapation of Karpathy's [code](https://github.com/karpathy/char-rnn) based on his [blog post](http://karpathy.github.io/2015/05/21/rnn-effectiveness/). It is a multi-layer Recurrent Neural Network using [Caffe](http://caffe.berkeleyvision.org/) for training/sampling from character-level language models. The main component of the network is a LSTM (Long Short-Term Memory) layer.

## Requirements

The only requirement for this code is Caffe and all its dependencies. The instruction to get them are on [Caffe installation page](http://caffe.berkeleyvision.org/installation.html).

Training with GPU (if you have CUDA installed along with Caffe) is supported and much faster than with CPU.

I have only tested this code on Windows with Visual Studio 2015, but it should also be able to run on Linux and OS X provided that Caffe is correctly installed.

## Usage

The project uses CMake as a building tool.
Once you have correctly built and compiled the code, you should be abble to launch the program.

### Sampling Shakespeare

The folder `launch_files` contains everything you need to make the model generate some Shakespeare or to train new networks. Just go in this folder and launch `test.bat` (or the corresponding line if you are not on Windows). The path to the application may also be slightly different if you are not on Windows.

### Training a new model

To train a new model, you only need one txt file with all your training samples (similar to the `shakespeare.txt` file). Then, you just have to launch `train.bat` with correct parameters.
If you change `sequence_length` or `batch_size` parameters, you will also have to change them in `caffe_char_rnn.prototxt`.

Just after the first launch of the program with your new text file, a new `vocabulary_XXX.txt` file is created. If the size of the vocabulary is different from the previous one, the program will crash. So you need to change it in `caffe_char_rnn.prototxt` and in `train.bat`. Once it is done, the program should run.

### Sampling with a new model

To generate new characters with a model, you need two files :

- a `vocabulary_XXX.txt` file with all the characters in your training set
- a `caffe_char_rnn_iter_XXXXXX.caffemodel` file with the trained network

Both of these files are created when you have trained your model. Once you have set these files in the `test.bat` file and you have set all the parameters according to what you want, you just can launch `test.bat` to generate the characters.

## Parameters

There are several parameters you can set to change the training/sampling.

### Training parameters

**snapshot**: use this with a `caffe_char_rnn_iter_XXXX.solverstate` file if you want to continue a training session

**logfile** and **log_interval**: if logfile is not empty, the loss will be saved every log_interval iteration. Use them to monitor your training and check if your network is not overfitting

**textfile**: the training dataset

### Testing parameters

**temperature**: the temperature is in range [0,1]. With higher temperature, the model is less conservative, but do more mistakes. If you set the temperature to 0, the model always takes the characters with the highest probability whithout considering the others even if they have very close probabilities.

**seed**: you can pass here a string that the model will use to initialize it's prediction. If the seed is shorter than sequence_length or if it is empty, random characters are added at the begining

**output_file**: use this parameter if you want to keep the predicted characters in a file

## Results

Here are some samples generated by the network trained on the `shakespeare.txt` file with the seed "To be, or not to be: that is the question:". Notice that network has learned the structure of the text and is abble to generate both dialogue and monologue (for this example the temperature was 0.5).

> To be, or not to be: that is the question:
My lord of Rome, such as he to the power
And when the sea of the day would tell them here.

>KING HENRY V:
Thou canst not see thee to my wife,
That the sea of the world and promise is this
That have of hand, as shown in the earth for me.

>BAPTISTA:
I have been a morning better to the other than this
as any thing of my life; that's the state of the company,
To be a barbarous head of sight of this
That she had land a tray to direct and with me.

>CASSIO:
A good sing, sir, I would not speak at his lady:
And there is the gods and the man bear him as I have
so long with the house and hold to him a good shape.

>ANTONIO:
That is the soul, why should he say the world?

>PRINCE HENRY:
I must not wear their way to wear you and the manner of a
company of the prince and the pains of your own account.

>SICINIUS:
Why, then, sir, I will do the fault of the day,
I am a daughter to the strong and meet.

>PANDARUS:
Pray you, my lord, that she is private through the end
and the trick of such a fool, and the father shall
make before the poor fall to the present man of my man:
And that the love to this hand for this incle,
That we have in my service of the world,
I would have washed and make her to my rest:
But now the strange and good blood say the worst
Is not a good more standing but to thee be plainly
To have it to the summer to the sun,
And will be satisfied to out the way:
The hand-proceeding countrymen are not a fool
Than this hour of the sea of his man

## License

MIT