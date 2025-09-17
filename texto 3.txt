const CACHE_NAME = 'pesagem-v1';
const urlsToCache = [
  '/',
  '/index.html',
  '/manifest.json',
  // URLs de bibliotecas e recursos
  'https://cdn.tailwindcss.com',
  'https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js',
  'https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js',
  'https://placehold.co/180x180/green/white?text=LF',
  'https://placehold.co/32x32/green/white?text=LF',
  'https://placehold.co/16x16/green/white?text=LF',
  'https://placehold.co/192x192/green/white?text=LF',
  'https://placehold.co/512x512/green/white?text=LF'
];

self.addEventListener('install', event => {
  // Aconselha o Service Worker a esperar a instalação
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => {
        console.log('Cache aberto');
        return cache.addAll(urlsToCache);
      })
  );
});

self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        // Retorna o cache se ele for encontrado
        if (response) {
          return response;
        }
        // Se não, busca na rede
        return fetch(event.request);
      })
  );
});

