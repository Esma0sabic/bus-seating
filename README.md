<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Bus Seat Reservation</title>
  <style>
    body {
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }
    .bus {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .row {
      display: flex;
      justify-content: center;
      gap: 20px;
    }
    .seats {
      display: flex;
      gap: 5px;
    }
    .seat {
      width: 40px;
      height: 40px;
      background-color: #eee;
      border: 1px solid #ccc;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
    }
    .taken {
      background-color: #ccc;
      cursor: not-allowed;
    }
    .door {
      height: 40px;
      margin: 5px;
      text-align: center;
      color: white;
      background: black;
      padding: 5px;
      font-size: 12px;
    }
  </style>
</head>
<body>
  <h1>Bus Seat Reservation</h1>
  <div class="bus" id="bus"></div>

  <script>
    const seatLayout = [
      [2, 2], [2, 2], [2, 2], [2, 2], [2, 2], // Rows 1–5
      [2, 2], [2, 2], [2, 2], [2, 2],         // Rows 6–9
      [2, 2], [2, 2], [2, 2], [2, 2],         // Rows 10–13
      [5]                                     // Row 14 (last row with 5 seats)
    ];

    const storageKey = 'bus-seat-reservations';
    const savedSeats = JSON.parse(localStorage.getItem(storageKey) || '{}');

    const bus = document.getElementById('bus');
    let seatNumber = 1;

    seatLayout.forEach((row, index) => {
      const rowDiv = document.createElement('div');
      rowDiv.classList.add('row');

      if (index === 1 || index === 8) {
        const door = document.createElement('div');
        door.classList.add('door');
        door.textContent = 'DOOR';
        rowDiv.appendChild(door);
      }

      row.forEach(side => {
        const seatsDiv = document.createElement('div');
        seatsDiv.classList.add('seats');

        for (let i = 0; i < side; i++) {
          const seat = document.createElement('div');
          seat.classList.add('seat');

          const seatKey = `seat${seatNumber}`;
          if (savedSeats[seatKey]) {
            seat.classList.add('taken');
            seat.textContent = savedSeats[seatKey];
          } else {
            seat.textContent = seatNumber;
            seat.addEventListener('click', () => {
              const name = prompt(`Enter your name to reserve seat ${seatNumber}`);
              if (name) {
                seat.textContent = name;
                seat.classList.add('taken');
                seat.removeEventListener('click', this);
                savedSeats[seatKey] = name;
                localStorage.setItem(storageKey, JSON.stringify(savedSeats));
              }
            });
          }

          seatsDiv.appendChild(seat);
          seatNumber++;
        }

        rowDiv.appendChild(seatsDiv);
      });

      bus.appendChild(rowDiv);
    });
  </script>
</body>
</html>
