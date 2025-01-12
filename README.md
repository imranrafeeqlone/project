import { useState } from 'react'
import { Button } from "/components/ui/button"
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "/components/ui/card"
import { Input } from "/components/ui/input"
import { Label } from "/components/ui/label"
import { RadioGroup, RadioGroupItem } from "/components/ui/radio-group"
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"
import { Textarea } from "@/components/ui/textarea"
import { Calendar } from "lucide-react"
import { format } from 'date-fns'

export default function QuickDevBookingPlatform() {
  const [developers, setDevelopers] = useState([
    { id: 1, name: 'John Doe', availability: 'online', hourlyRate: 50, bookedSlots: [] },
    { id: 2, name: 'Jane Smith', availability: 'offline', hourlyRate: 60, bookedSlots: [] },
  ])

  const [selectedDeveloper, setSelectedDeveloper] = useState<number | null>(null)
  const [bookingTime, setBookingTime] = useState('')

  const handleDeveloperChange = (id: number) => {
    setSelectedDeveloper(id)
  }

  const handleTimeChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setBookingTime(event.target.value)
  }

  const handleBook = () => {
    if (selectedDeveloper !== null && bookingTime) {
      const updatedDevelopers = developers.map(dev => {
        if (dev.id === selectedDeveloper) {
          return {
            ...dev,
            bookedSlots: [...dev.bookedSlots, bookingTime]
          }
        }
        return dev
      })
      setDevelopers(updatedDevelopers)
      setSelectedDeveloper(null)
      setBookingTime('')
    }
  }

  return (
    <div className="min-h-screen bg-white flex flex-col">
      <header className="bg-primary text-primary-foreground shadow-lg">
        <div className="container mx-auto px-4 py-6 flex justify-between items-center">
          <h1 className="text-2xl font-bold">Quick Dev Booking Platform</h1>
        </div>
      </header>

      <main className="flex-grow container mx-auto px-4 py-8">
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <Card>
              <CardHeader>
                <CardTitle>Developer Profiles</CardTitle>
              </CardHeader>
              <CardContent>
                {developers.map(dev => (
                  <div key={dev.id} className="mb-4">
                    <div className="flex justify-between items-center">
                      <div>
                        <p className="text-lg font-semibold">{dev.name}</p>
                        <p className="text-sm text-muted-foreground">{dev.availability === 'online' ? 'Online' : 'Offline'}</p>
                      </div>
                      <p className="text-sm text-muted-foreground">${dev.hourlyRate}/hr</p>
                    </div>
                    <div className="mt-2">
                      <Button
                        variant="outline"
                        onClick={() => handleDeveloperChange(dev.id)}
                        disabled={dev.availability === 'offline'}
                      >
                        {selectedDeveloper === dev.id ? 'Selected' : 'Select'}
                      </Button>
                    </div>
                  </div>
                ))}
              </CardContent>
            </Card>
          </div>
          <div>
            <Card>
              <CardHeader>
                <CardTitle>Book a Developer</CardTitle>
              </CardHeader>
              <CardContent>
                {selectedDeveloper !== null ? (
                  <>
                    <div className="mb-4">
                      <Label htmlFor="time">Select Time</Label>
                      <Input
                        id="time"
                        type="datetime-local"
                        value={bookingTime}
                        onChange={handleTimeChange}
                        className="mt-2"
                      />
                    </div>
                    <Button onClick={handleBook} disabled={!bookingTime}>
                      Book
                    </Button>
                  </>
                ) : (
                  <p className="text-muted-foreground">Please select a developer to book.</p>
                )}
              </CardContent>
            </Card>
            <Card className="mt-6">
              <CardHeader>
                <CardTitle>Booked Slots</CardTitle>
              </CardHeader>
              <CardContent>
                {selectedDeveloper !== null ? (
                  developers.find(dev => dev.id === selectedDeveloper)?.bookedSlots.map(slot => (
                    <div key={slot} className="mb-2">
                      <p>{format(new Date(slot), 'yyyy-MM-dd HH:mm')}</p>
                    </div>
                  ))
                ) : (
                  <p className="text-muted-foreground">No slots booked yet.</p>
                )}
              </CardContent>
            </Card>
          </div>
        </div>
      </main>

      <footer className="bg-muted mt-8">
        <div className="container mx-auto px-4 py-6 text-center">
          <p>&copy; 2023 Quick Dev Booking Platform. All rights reserved.</p>
        </div>
      </footer>
    </div>
  )
}
