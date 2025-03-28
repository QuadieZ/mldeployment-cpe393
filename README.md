# ML Model Deployment API

This API provides endpoints for two machine learning models:

1. Iris Flower Classification
2. House Price Prediction

## Setup

1. Create models & Build Docker image:

```bash
python train.py
python house_price.py
docker build -t ml-model .
```

2. Run Docker container:

```bash
docker run -p 9000:9000 ml-model
```

## API Endpoints

### 1. Health Check

Check if the API is running.

**Endpoint:** `GET /health`

**Response:**

```json
{
  "status": "OK!"
}
```

### 2. Iris Flower Classification

Predict the species of an iris flower based on its measurements.

**Endpoint:** `POST /predict`

**Request Body:**

```json
{
  "features": [
    [5.1, 3.5, 1.4, 0.2],
    [6.2, 3.4, 5.4, 2.3]
  ]
}
```

**Features:**

- Sepal length (cm)
- Sepal width (cm)
- Petal length (cm)
- Petal width (cm)

**Response:**

```json
{
  "predictions": [
    {
      "prediction": 0,
      "confidence": 0.97
    },
    {
      "prediction": 2,
      "confidence": 0.95
    }
  ]
}
```

**Prediction Values:**

- 0: Setosa
- 1: Versicolor
- 2: Virginica

### 3. House Price Prediction

Predict the price of a house based on various features.

**Endpoint:** `POST /predict_house`

**Request Body:**

```json
{
  "features": [
    9960,
    3,
    2,
    2,
    "yes",
    "no",
    "yes",
    "no",
    "no",
    2,
    "yes",
    "semi-furnished"
  ]
}
```

**Features (in order):**

1. area (sq ft)
2. bedrooms (number)
3. bathrooms (number)
4. stories (number)
5. mainroad (yes/no)
6. guestroom (yes/no)
7. basement (yes/no)
8. hotwaterheating (yes/no)
9. airconditioning (yes/no)
10. parking (number)
11. prefarea (yes/no)
12. furnishingstatus (furnished/semi-furnished/unfurnished)

**Response:**

```json
{
  "predicted_price": 1234567.89
}
```

## Example Usage

### Iris Classification

```bash
curl -X POST http://localhost:9000/predict \
     -H "Content-Type: application/json" \
     -d '{
       "features": [5.1, 3.5, 1.4, 0.2]
     }'
```

### House Price Prediction

```bash
curl -X POST http://localhost:9000/predict_house \
     -H "Content-Type: application/json" \
     -d '{
       "features": [
         9960, 3, 2, 2, "yes", "no", "yes", "no", "no", 2, "yes", "semi-furnished"
       ]
     }'
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

- 400: Bad Request (invalid input format)
- 500: Internal Server Error

Example error response:

```json
{
  "error": "Invalid Input: Features must be a list"
}
```
