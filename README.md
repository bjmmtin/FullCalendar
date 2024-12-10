# FullCalendar

```
import dayGridPlugin from "@fullcalendar/daygrid/index.js";
import interactionPlugin from "@fullcalendar/interaction/index.js";
import FullCalendar from "@fullcalendar/react";
import timeGridPlugin from "@fullcalendar/timegrid/index.js";
import { useState } from "react";

// Mockup data for bookings
const mockBookings = [
  {
    _id: "1",
    property: { address: "123 Main St" },
    contact: { firstName: "John", lastName: "Doe" },
    startTime: 1672531200, // Unix timestamp
    localTimeZone: "America/Los_Angeles",
    status: "confirmed",
  },
  {
    _id: "2",
    property: { address: "456 Elm St" },
    contact: { firstName: "Jane", lastName: "Smith" },
    startTime: 1672617600,
    localTimeZone: "America/Los_Angeles",
    status: "archived",
  },
  {
    _id: "3",
    property: { address: "789 Oak St" },
    contact: { firstName: "Alice", lastName: "Johnson" },
    startTime: 1672704000,
    localTimeZone: "America/Los_Angeles",
    status: "confirmed",
  },
];

const customizeFullCalendar = () => {
  const [initialDate, setInitialDate] = useState(() => {
    const currentDate = new Date();
    return toZonedTime(currentDate, "America/Los_Angeles"); // Ensure this function is defined
  });

  const renderEventContent = (eventInfo) => {
    return (
      <>
        {eventInfo.view.type === "timeGridDay" ? (
          <div
            className={eventInfo.event.classNames.join(' ')}
            style={{
              borderLeft: `3px solid ${eventInfo.borderColor}`,
              borderRadius: "3px",
              boxShadow: "0 0 0 1px white",
              lineHeight: "normal",
              padding: "6px",
              overflow: "hidden",
            }}
          >
            <div className="fc-event-title text-nowrap" style={{ paddingBottom: "3px" }}>
              {eventInfo.event.title}
            </div>
            <div className="d-flex">
              <div className="fc-event-time text-nowrap">
                {new Date(eventInfo.event.startStr).toLocaleString("en-US", {
                  hour: "numeric",
                  minute: "numeric",
                  hour12: true,
                })}
              </div>
              <div className="fc-event-title text-nowrap">
                {eventInfo.event.extendedProps?.name}
              </div>
            </div>
          </div>
        ) : (
          <div className="d-flex align-items-center" style={{
            overflow: "hidden",
            boxShadow: "0 0 0 1px white",
            width: "100%",
            borderRadius: "3px",
          }}>
            <div className="fc-daygrid-event-dot mx-1" style={{ color: `${eventInfo.borderColor}` }}></div>
            <div className="fc-event-time text-center" style={{ lineHeight: "25px" }}>
              {new Date(eventInfo.event.startStr).toLocaleString("en-US", {
                hour: "numeric",
                minute: "numeric",
                hour12: true,
              })} {eventInfo.event.title}
            </div>
          </div>
        )}
      </>
    );
  };

  return (
    <FullCalendar
      key={initialDate.toString()}
      height={`${window.innerWidth > 755 ? "100vh" : "800px"}`}
      plugins={[dayGridPlugin, timeGridPlugin, interactionPlugin]}
      initialView="dayGridMonth"
      headerToolbar={{
        left: "title",
        center: "",
        right: "today,timeGridDay,timeGridWeek,dayGridMonth,prev,next",
      }}
      selectMirror={true}
      dayMaxEvents={true}
      themeSystem="Simplex"
      eventClick={(calEvent) => handleClickBooking(calEvent.event.id)} // Ensure this function is defined
      eventContent={(eventInfo) => renderEventContent(eventInfo)}
      events={mockBookings.map((booking) => ({
        id: booking._id,
        title: booking.property?.address || 'No Address',
        description: booking.property?.address || 'No Description',
        extendedProps: {
          name: `${booking.contact.firstName} ${booking.contact.lastName}`,
        },
        start: toZonedTime(new Date(booking.startTime * 1000), booking.localTimeZone),
        end: toZonedTime(new Date((booking.startTime + 15 * 60) * 1000), booking.localTimeZone),
        classNames: booking.status === "archived" ? "fc-event-bg-default" : "fc-event-bg-default",
        borderColor: booking.status === "archived" ? "#646464" : "#007bff",
        textColor: "#1F2327",
      }))}
      dateClick={(info) => console.log({ info })}
      initialDate={initialDate}
      eventTimeFormat={{
        hour: "numeric",
        minute: "2-digit",
      }}
      dayHeaderFormat={{
        weekday: "short",
      }}
    />
  );
}

```
