mkdir build && cd build

cmake -DLLAMA_METAL=ON -DLLAMA_CURL=ON ..

cmake --build . --config Release -t llama-server

./build/bin/llama-server -m models/dolphin-2.2.1-mistral-7b.Q2_K.gguf -c 32768 --n-gpu-layers 24 -fa

---

cmake -S . -B build -G Ninja -DGGML_VULKAN=ON -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/your/install/dir -DLLAMA_BUILD_TESTS=OFF -DLLAMA_BUILD_EXAMPLES=ON -DLLAMA_BUILD_SERVER=ON

cmake --build build --config Release -j 16

./build-amd/bin/llama-server -m models/dolphin-2.2.1-mistral-7b.Q2_K.gguf -c 32768 --n-gpu-layers 24 -fa

---

mkdir build0fast && cd build0fast

cmake \
  -DGGML_METAL=OFF \
  -DLLAMA_CURL=ON \
  -DCMAKE_C_FLAGS="-Ofast -march=native -funroll-loops -ffast-math -fno-finite-math-only" \
  -DCMAKE_CXX_FLAGS="-Ofast -march=native -funroll-loops -ffast-math -fno-finite-math-only" \
  ..

cmake --build . --config Release -t llama-server

./build0fast/bin/llama-server -m models/dolphin-2.2.1-mistral-7b.Q2_K.gguf -c 4096 --n-gpu-layers 0 -fa

---

mkdir build0fast0furious && cd build0fast0furious

```sh
cmake \
  -DCMAKE_C_FLAGS="-Ofast -march=native -mavx2 -mfma -flto" \
  -DCMAKE_CXX_FLAGS="-Ofast -march=native -mavx2 -mfma -flto" \
  -DLLAMA_CURL=ON \
  ..
```

cmake --build . --config Release -j8 --target llama-server

./build0fast0furious/bin/llama-server -m models/dolphin-2.2.1-mistral-7b.Q2_K.gguf -c 4096 -ngl 0
