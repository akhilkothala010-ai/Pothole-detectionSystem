# CrowdSafe Road Platform - Setup Instructions

## Database Setup

The database schema has already been created with the following tables:
- `potholes` - Stores pothole reports with images and locations
- `pothole_ratings` - Stores community ratings for potholes
- `community_reports` - Stores general community issue reports
- `user_profiles` - Extended user profile information

## Storage Bucket Setup

You need to create a storage bucket for pothole images:

1. Go to your Supabase Dashboard
2. Navigate to **Storage** in the left sidebar
3. Click **New bucket**
4. Create a bucket named: `images`
5. Set it as **Public** bucket (so uploaded images can be viewed)
6. Click **Create bucket**

## Storage Policies

After creating the bucket, you need to set up storage policies:

1. Click on the `images` bucket
2. Go to **Policies**
3. Add the following policies:

### Allow authenticated users to upload:
```sql
CREATE POLICY "Authenticated users can upload images"
ON storage.objects FOR INSERT
TO authenticated
WITH CHECK (bucket_id = 'images');
```

### Allow public access to view images:
```sql
CREATE POLICY "Public can view images"
ON storage.objects FOR SELECT
TO public
USING (bucket_id = 'images');
```

### Allow users to delete their own uploads:
```sql
CREATE POLICY "Users can delete own images"
ON storage.objects FOR DELETE
TO authenticated
USING (bucket_id = 'images' AND auth.uid()::text = (storage.foldername(name))[1]);
```

## Running the Application

1. Make sure your `.env` file has the correct Supabase credentials
2. Run `npm install` to install dependencies
3. Run `npm run dev` to start the development server
4. Open your browser to the provided URL

## Features

- User authentication with email/password
- Report potholes with image upload and GPS location
- Community rating system for pothole verification
- Interactive map view of all reported potholes
- Nearby alerts based on user location
- Community reports for other road issues
- About page with platform information

## Note

The application uses GPS location services. Users will be prompted to allow location access for features like:
- Detecting location when reporting potholes
- Viewing nearby alerts
- Pinpointing community report locations
