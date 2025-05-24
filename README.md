# Gp
Ggpp
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout, Flatten
from tensorflow.keras.datasets import mnist
from tensorflow.keras.utils import to_categorical

def build_model(input_shape, num_classes, hidden_layers=[128, 64], dropout_rate=0.2):
    model = Sequential()
    model.add(Flatten(input_shape=input_shape))
    
    for units in hidden_layers:
        model.add(Dense(units, activation='relu'))
        model.add(Dropout(dropout_rate))
    
    model.add(Dense(num_classes, activation='softmax'))
    return model

def train_model(model, x_train, y_train, x_test, y_test, epochs=10, batch_size=128):
    model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
    model.fit(x_train, y_train, validation_data=(x_test, y_test), epochs=epochs, batch_size=batch_size)
    test_loss, test_acc = model.evaluate(x_test, y_test)
    print(f"Test Accuracy: {test_acc:.4f}")

def main():
    # Load MNIST data
    (x_train, y_train), (x_test, y_test) = mnist.load_data()
    x_train = x_train.astype("float32") / 255.0
    x_test = x_test.astype("float32") / 255.0
    y_train = to_categorical(y_train, 10)
    y_test = to_categorical(y_test, 10)

    # Define different architectures here
    architectures = [
        [128],              # Simple one hidden layer
        [256, 128],         # Two hidden layers
        [512, 256, 128],    # Three hidden layers
    ]

    for i, arch in enumerate(architectures, 1):
        print(f"\nTraining Model {i} with architecture: {arch}")
        model = build_model(input_shape=(28, 28), num_classes=10, hidden_layers=arch)
        train_model(model, x_train, y_train, x_test, y_test)

if __name__ == "__main__":
    main()
