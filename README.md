# Autoencoder untuk Dataset Gambar

Proyek ini mengimplementasikan model Autoencoder untuk memproses dataset gambar kustom, di mana gambar input dan target berbeda. Tujuannya adalah untuk melatih Autoencoder agar dapat mempelajari representasi terkompresi dari gambar dan menghasilkan gambar output yang mirip dengan gambar target.

## Dataset

Dataset yang digunakan dalam proyek ini terdiri dari gambar kucing dan anjing. Gambar-gambar tersebut diorganisir dalam struktur berikut:

- **Gambar Input (kucing)**: `/content/dataset/test_set/test_set/cats/`
- **Gambar Target (anjing)**: `/content/dataset/test_set/test_set/dogs/`

Gambar-gambar tersebut diubah ukurannya menjadi resolusi 128x128 dan dikonversi menjadi grayscale untuk gambar input. Gambar target tetap dalam format RGB aslinya.

## Arsitektur Model

Model Autoencoder yang digunakan terdiri dari komponen-komponen berikut:

1. **Encoder**:
    - Lapisan konvolusional yang melakukan downsampling gambar.
    - Lapisan linier untuk lebih mengurangi dimensi gambar.

2. **Bottleneck (Representasi Latent)**:
    - Representasi kompak dan terkompresi dari gambar input.

3. **Decoder**:
    - Sekumpulan lapisan konvolusional terbalik yang membangun kembali gambar dari representasi latent.

Model ini dilatih untuk meminimalkan perbedaan antara gambar output yang direkonstruksi dan gambar target menggunakan fungsi loss Mean Squared Error (MSE).

## Pelatihan

Untuk melatih model, langkah-langkah berikut dilakukan:

1. Menentukan transformasi untuk gambar input dan target (mengubah ukuran, mengonversi menjadi tensor, dan normalisasi).
2. Memuat dataset ke dalam kelas `Dataset` kustom yang memasangkan gambar input dan target.
3. Melatih model Autoencoder menggunakan dataset dengan optimizer standar (Adam) dan fungsi loss (MSE loss).

### Kode Pelatihan:

```python
# Contoh loop pelatihan

for epoch in range(num_epochs):
    running_loss = 0.0
    for i, (input_images, target_images) in enumerate(dataloader):
        # Forward pass
        output_images = model(input_images)
        
        # Hitung loss
        loss = criterion(output_images, target_images)
        
        # Backpropagation
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        
        running_loss += loss.item()
    
    print(f'Epoch [{epoch+1}/{num_epochs}], Loss: {running_loss/len(dataloader)}')
